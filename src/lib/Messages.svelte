<script lang="ts">
    import { onMount, onDestroy } from "svelte";
    import { currentUser, pb } from "./pocketbase";

    let newMessage: string;
    let messages: any[] = [];
    let unsubscribe: () => void;

    onMount(async () => {
        const resultList = await pb.collection('chaos_messages').getList(1, 50, {
            sort: 'created',
            expand: 'user',
        }); 
        messages = resultList.items;

        unsubscribe = await pb
            .collection('chaos_messages')
            .subscribe('*', async ({ action, record}) => {
                if (action === 'create') {
                    const user = await pb.collection('users').getOne(record.user);
                    record.expand = { user };
                    messages = [...messages, record];
                }

                if (action === 'delete') {
                    messages = messages.filter((m) => m.id !== record.id);
                }

            });
    });

    onDestroy(() => {
        unsubscribe?.();
    });

    async function sendMessage() {
        const data = {
            text: newMessage,
            user: $currentUser.id,
        };

        const createdMessage = await pb.collection('chaos_messages').create(data);
        newMessage = '';
    }

</script>

<div class="section">
    <h1>
        Messages
    </h1>
</div>

<div class="messages">
    {#each messages as message (message.id)}
        <div class="msg">
            <img 
                class="avatar"
                src={'https://api.dicebear.com/7.x/adventurer/svg?seed=${message.expand?.user?.username}'}
                alt="avatar"
                width="40px"
            />
            <div>
                <small>
                    Sent by @{message.expand?.user?.username}
                </small>
                <p class="msg-text">{message.text}</p>
            </div>
        </div>
    {/each}
</div>

<form on:submit|preventDefault={sendMessage}>
    <input placeholder="Message" type="text" bind:value={newMessage} />
    <button type="submit">Send</button>
</form>

<style>
    div {
        display: block;
    }
    .messages{
        border: 1px solid #ddd;
        overflow: auto;
        max-height: 400px;
        text-align: left;
        padding: 10px;
    }
</style>