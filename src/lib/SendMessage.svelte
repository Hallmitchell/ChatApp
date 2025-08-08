<script lang="ts">
    import { onMount, onDestroy } from 'svelte';
    import { currentUser, pb } from './pocketbase';
    export let userIdToSendMessageTo = null;
    let newMessage: string;
    let messages: any[] = [];
    let unsubscribe: () => void;

    async function sendMessage() {
        const table = userIdToSendMessageTo ? "directMessages" : "messages"
        const data = {
            text: newMessage,
        };
        if (userIdToSendMessageTo){
            data["userSender"] = $currentUser.id
            data["userReciever"] = userIdToSendMessageTo
        } else {
            data["user"] = $currentUser.id
        }
        const createdMessage = await pb.collection(table).create(data);
        newMessage = "";
    }

</script>

<form on:submit|preventDefault={sendMessage}>
    <input placeholder="Message" type="text" bind:value={newMessage} />
    <button type="submit">Send</button> 
</form>
