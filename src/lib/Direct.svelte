<script lang="ts">
    import { onMount, onDestroy } from 'svelte';
    import { currentUser, pb } from './pocketbase';
    import ReusableMessages from './ReusableMessages.svelte';
    let newMessage: string;
    let messages: any[] = [];
    let directMessages: any = {};
    let currentSelectedUser: any = null;
    let unsubscribe: () => void;

    onMount( async() => {
        console.log("currentUser", $currentUser?.id)
        const resultList = await pb.collection('directMessages').getList(1, 50, {
            sort:'created',
            filter:`userSender = "${$currentUser?.id}" || userReciever = "${$currentUser?.id}"`,
            expand: 'userSender,userReciever',
        })
        messages = resultList.items;
        unsubscribe = await pb  
                    .collection('directMessages')
                    .subscribe('*', async ({ action, record }) => {
                        if (action === 'create') {
                            console.log("action", action, record);
                            const userReciever = await pb.collection('users').getOne(record.userReciever);
                            const userSender = await pb.collection('users').getOne(record.userReciever);
                            record.expand = { userReciever, userSender };
                             if (record.expand.userSender.id !== $currentUser?.id) {
                                if (record.expand.userSender.userName in directMessages){
                                    directMessages[record.expand.userSender.userName].push(record)
                                } else {
                                    directMessages[record.expand.userSender.userName] = [record]
                                }
                            }
                            if (record.expand.userReciever.id !== $currentUser?.id) {
                                if (record.expand.userReciever.userName in directMessages){
                                    directMessages[record.expand.userReciever.userName].push(record)
                                } else {
                                    directMessages[record.expand.userReciever.userName] = [record]
                                }
                            }
                          
                            // messages = [...messages, record];
                        }
                        if (action === 'delete') {
                            messages = messages.filter((m) => m.id !== record.id);
                        }
                    })

        console.log("resultList!", directMessages);
        messages.map((message) => {
            if (message.userSender !== $currentUser?.id) {
                if (message.expand.userSender.userName in directMessages){
                    directMessages[message.expand.userSender.userName].push(message)
                } else {
                    directMessages[message.expand.userSender.userName] = [message]
                }
            }
            if (message.userReciever !== $currentUser?.id) {
                if (message.expand.userReciever.userName in directMessages){
                    directMessages[message.expand.userReciever.userName].push(message)
                } else {
                    directMessages[message.expand.userReciever.userName] = [message]
                }
            }
            
        })
    })
</script>

<label for="Users">Choose a user</label>

<select name="Users" bind:value={currentSelectedUser}>
    {#each Object.keys(directMessages) as user}
        <option value={user}>@{user}</option>
    {/each}

</select>
 {#if currentSelectedUser}
    <ReusableMessages messages={directMessages[currentSelectedUser]}/>
{/if}
<!-- <p>{currentSelectedUser}</p> -->