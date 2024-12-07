<script>
    import { goto } from '$app/navigation';
  
    let email = '';
    let password = '';
  
    const handleLogin = () => {
      if (email && password) {
        
      } else {
        alert('Please enter a valid email and password.');
      }
    };
  </script>
  
  <div class="flex justify-center items-center min-h-screen bg-gradient-to-br from-orange-100 via-white to-orange-50">
    <!-- Login Card -->
    <div class="bg-white shadow-2xl rounded-lg p-8 max-w-md w-full border-t-4 border-orange-500">
      <!-- Logo and Company Name -->
      <div class="text-center mb-8">
        <img src="/logo.jpeg" alt="SR Automation Logo" class="mx-auto w-20 h-20 mb-4" />
        <h1 class="text-4xl font-bold text-gray-800">SR Automation</h1>
        <p class="text-sm text-gray-600 font-medium">
          Streamline your automation with confidence.
        </p>
      </div>
  
      <!-- Login Form -->
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
            class="mt-1 block w-full px-4 py-3 border border-gray-300 rounded-lg shadow-sm focus:outline-none focus:ring-1 focus:ring-orange-500 focus:border-orange-500 placeholder-gray-400"
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
            class="mt-1 block w-full px-4 py-3 border border-gray-300 rounded-lg shadow-sm focus:outline-none focus:ring-1 focus:ring-orange-500 focus:border-orange-500 placeholder-gray-400"
            placeholder="Enter your password"
          />
        </div>
        <button
          type="submit"
          class="w-full bg-orange-600 text-white font-semibold py-3 px-4 rounded-lg shadow-lg hover:bg-orange-700 focus:outline-none focus:ring focus:ring-orange-300 transition"
        on:click={()=>goto('/mainpage')}
        >
          Login
        </button>
      </form>
  
      <!-- Footer -->
      <div class="mt-8 text-center text-sm text-gray-500">
        <p>
          Need help? <a href="#" class="text-orange-600 font-medium hover:underline">Contact Support</a>
        </p>
      </div>
    </div>
  </div>
  