<script lang="ts">
    import { currentUser, pb } from './pocketbase';

    let username: string;
    let password: string;

    async function login() {
        await pb.collection('users').authWithPassword(username, password);
    }

    async function signUp() {
        try {
            const data = {
                username,
                password,
                passwordConfirm: password,
                name: 'hi mom!',
            };
            const createdUser = await pb.collection('users').create(data);
            await login();
        } catch (err) {
            console.error(err);
        }
    }

    function signOut() {
        pb.authStore.clear();
    }
</script>

<div class="bg-gray-100 bg-[url('https://subtle-patterns.com/patterns/carbon_fibre.png')]">
    <!-- Navbar -->
    <nav class="bg-gray-800 p-4 flex justify-between items-center">
        <img src="/logo2.png" alt="logo" style="height:35px; width:30px;"/>
        <div class="text-white left-2 text-xl font-semibold text-center">Admin Dashboard</div>
        {#if $currentUser}
            <div class="text-white flex items-center">
                <p class="mr-4">Signed in as <span class="font-semibold">{$currentUser.username}</span></p>
                <button
                        on:click={signOut}
                        class="bg-gray-700 text-white py-2 px-4 rounded hover:bg-gray-600 transition duration-200"
                >
                    Sign Out
                </button>
            </div>
        {/if}
    </nav>

    <!-- Main Content -->
    {#if !$currentUser}
        <div class="flex items-center justify-center min-h-[calc(100vh-4rem)]">
            <div class="bg-white p-10 rounded-lg shadow-md w-112 max-w-full">
                <h1 class="font-bold text-3xl leading-tight text-center mb-8">Login to Admin Dashboard</h1>
                <form on:submit|preventDefault class="space-y-6">
                    <input
                            placeholder="Username"
                            type="text"
                            bind:value={username}
                            class="w-full border border-gray-300 rounded px-4 py-3 focus:outline-none focus:ring-2 focus:ring-gray-400"
                    />
                    <input
                            placeholder="Password"
                            type="password"
                            bind:value={password}
                            class="w-full border border-gray-300 rounded px-4 py-3 focus:outline-none focus:ring-2 focus:ring-gray-400"
                    />
                    <div class="flex space-x-4">
                        <button
                                on:click={login}
                                class="flex-1 bg-gray-600 text-white py-3 px-4 rounded hover:bg-gray-500 transition duration-200"
                        >
                            Login
                        </button>
                    </div>
                </form>
            </div>
        </div>
    {/if}
</div>