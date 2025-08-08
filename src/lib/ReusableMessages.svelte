<script lang="ts">
    import { onMount, onDestroy } from 'svelte';
    import { currentUser, pb } from './pocketbase';
    import SendMessage from './SendMessage.svelte';
    export let messages;    
</script>

<div class="messages" >
    {#each messages as message (message.id)}
        <div class="msg">
            <!-- <img
                class="avatar"
                src={`https://avatars.dicebear.com/api/identicon/${message.expand?.user?.username}.svg`}
                alt="avatar"
                width="40px"
                /> -->
            <div>
                {#if message.userSender === $currentUser.id }
                    <small style="color:orange; style=font-size:400%; text-align:right;">
                        <b>
                            <p style="text-align:right;">@{message.expand?.userSender?.userName}</p>
                        </b>
                    </small>
                    <p style="color:white; msg-text; text-align:right">{message.text}</p>
                {:else}
                     <small style="color:orange; style=font-size:400%; text-align:left;">
                        <b>
                            <p style="text-align-left;">@{message.expand?.userSender?.userName}</p>
                        </b>
                    </small>
                    <p style="color:black; text-align:left">{message.text}</p>

                {/if}
            </div>
        </div>
    {/each}
</div>
<SendMessage userIdToSendMessageTo={messages[0].expand.userSender.id === $currentUser?.id ? messages[0].expand.userReciever.id : messages[0].expand.userSender.id }/>
