<script>
    import { goto } from '$app/navigation';
    let email = '';
    let password = '';
    let loading = false;
    let errorMessage = '';
    let successMessage = '';
    let showError = false;
    let showSuccess = false;
  
    const handleLogin = async () => {
        loading = true;
        showError = false;
        showSuccess = false;
        errorMessage = '';
        successMessage = '';
  
        try {
            const response = await fetch("https://sr-backend-go.onrender.com/login", {
                method: "POST",
                body: JSON.stringify({ email, password }),
            });
  
            if (response.ok) {
                const data = await response.json();
                successMessage = "Login successfull !!!";
                showSuccess = true;
                setTimeout(() => {
                    goto("/login/" + data.message);
                }, 2000);
            } else {
                const jsonResponse = await response.json();
                errorMessage = jsonResponse.message || 'Invalid email or password.';
                showError = true;
            }
        } catch (error) {
            console.error("Error:", error);
            errorMessage = 'Failed to communicate with the server. Please try again later.';
            showError = true;
        } finally {
            loading = false;
            if (showError) {
                setTimeout(() => {
                    showError = false;
                }, 3000);
            }
        }
    };
  </script>
  
  <div class="flex justify-center items-center min-h-screen bg-gradient-to-br from-white via-white to-white">

    <div class="bg-white shadow-2xl rounded-lg p-8 max-w-md w-full border-t-4 border-black">

        <div class="text-center mb-8">
            <img src="/logo.jpeg" alt="SR Automation Logo" class="mx-auto w-20 h-20 mb-4" />
            <h1 class="text-4xl font-bold text-gray-800">SR Automation</h1>
            <p class="text-sm text-gray-600 font-medium">
                Streamline your automation with confidence.
            </p>
        </div>
  
    
        <div class="relative">
            {#if successMessage}
              <div class="fixed bottom-4 right-4 bg-black text-white px-4 py-2 rounded shadow-lg">
                {successMessage}
              </div>
            {/if}
          
            {#if errorMessage}
              <div class="fixed bottom-4 right-4  bg-black text-white px-4 py-2 rounded shadow-lg">
                {errorMessage}
              </div>
            {/if}
          
          
          </div>
  
       
        <form on:submit|preventDefault={handleLogin} class="space-y-6">
            <div>
                <label for="email" class="block text-sm font-medium text-gray-700">
                    Email Address
                </label>
                <input
                    id="email"
                    type="email"
                    bind:value={email}
                    required
                    class="mt-1 block w-full px-4 py-3 border border-gray-300 rounded-lg shadow-sm focus:outline-none focus:ring-1 focus:ring-black focus:border-black placeholder-gray-400"
                    placeholder="Enter your email"
                />
            </div>
            <div>
                <label for="password" class="block text-sm font-medium text-gray-700">
                    Password
                </label>
                <input
                    id="password"
                    type="password"
                    bind:value={password}
                    required
                    class="mt-1 block w-full px-4 py-3 border border-gray-300 rounded-lg shadow-sm focus:outline-none focus:ring-1 focus:ring-black focus:border-black placeholder-gray-400"
                    placeholder="Enter your password"
                />
            </div>
            <button
                type="submit"
                class="w-full flex justify-center items-center bg-black text-white font-semibold py-3 rounded-xl text-lg focus:outline-none focus:ring-2 focus:ring-offset-2 transition"
            >
                {#if loading}
                    <div class="animate-spin rounded-full h-6 w-6 border-t-4 border-white border-solid"></div>
                {:else}
                    Login
                {/if}
            </button>
        </form>
  

        <div class="mt-8 text-center text-sm text-gray-500">
            <p>
                Don't have an account? 
                <button
                    class="text-black font-medium hover:underline"
                    on:click={() => goto('/register')}
                >
                    Register
                </button>
            </p>
        </div>
    </div>
  </div>
  