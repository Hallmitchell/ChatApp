<script lang="ts">
    import { onMount, onDestroy } from 'svelte';
    import { currentUser, pb } from './pocketbase';
    let newMessage: string;
    let messages: any[] = [];
    let unsubscribe: () => void;

    onMount(async () => {
        // Get initial messages
        const resultList = await pb.collection('messages').getList(1, 50, {
            sort: 'created',
            expand: 'user',
        });
        messages = resultList.items;

        unsubscribe = await pb  
            .collection('messages')
            .subscribe('*', async ({ action, record }) => {
                if (action === 'create') {
                    const user = await pb.collection('users').getOne(record.user);
                    record.expand = { user };
                    messages = [...messages, record];
                }
                if (action === 'delete') {
                    messages = messages.filter((m) => m.id !== record.id);
                }
            })
    });

    onDestroy(() => {
        unsubscribe?.();
    });

    async function sendMessage() {
        const data = {
            text: newMessage,
            user: $currentUser.id,
        };
        const createdMessage = await pb.collection('messages').create(data);
        newMessage = "";
    }

</script>

<div class="messages" style="width:140px; margin:auto;">
    {#each messages as message (message.id)}
        <div class="msg">
            <!-- <img
                class="avatar"
                src={`https://avatars.dicebear.com/api/identicon/${message.expand?.user?.username}.svg`}
                alt="avatar"
                width="40px"
                /> -->
            <div>
                {#if message.user === $currentUser.id }
                    <small style="color:orange; style=font-size:400%; text-align:right;">
                        <b>
                            <p style="text-align:right;">@{message.expand?.user?.userName}</p>
                        </b>
                    </small>
                    <p style="color:white; msg-text; text-align:right">{message.text}</p>
                {:else}
                     <small style="color:orange; style=font-size:400%; text-align:left;">
                        <b>
                            <p style="text-align-left;">@{message.expand?.user?.userName}</p>
                        </b>
                    </small>
                    <p style="color:black; text-align:left">{message.text}</p>

                {/if}
            </div>
        </div>
    {/each}
</div>

<form on:submit|preventDefault={sendMessage}>
    <input placeholder="Message" type="text" bind:value={newMessage} />
    <button type="submit">Send</button>
    
</form>
