<script>
  import { goto } from '$app/navigation';
  import { onDestroy } from 'svelte';

  let email = '';
  let password = '';
  let confirmPassword = '';
  let loading = false;
  let errorMessage = '';
  let showError = false;
  let timer;

  const handleRegister = async () => {
    if (!email || !password || !confirmPassword) {
      errorMessage = 'Please fill out all fields.';
      showError = true;
      resetError();
      return;
    }

    if (password !== confirmPassword) {
      errorMessage = 'Passwords do not match.';
      showError = true;
      resetError();
      return;
    }

    loading = true;
    errorMessage = '';
    showError = false;

    try {
      const response = await fetch("http://localhost:8000/register", {
        method: 'POST',
        
        body: JSON.stringify({
          "email": email,
          "password": password,
          "confirm_password": confirmPassword
        })
      });

      if (response.ok) {
    
        goto('/login');
      } else {
        const jsonResponse = await response.json();
        errorMessage = jsonResponse.message || response.statusText || 'An unexpected error occurred.';
        showError = true;
      }
    } catch (error) {
      console.error('Error:', error);
      errorMessage = 'Failed to communicate with the server. Please try again later.';
      showError = true;
    } finally {
      loading = false;
      resetError();
    }
  };

  const resetError = () => {
    timer = setTimeout(() => (showError = false), 3000);
  };

  onDestroy(() => {
    if (timer) clearTimeout(timer);
  });
</script>

<div class="flex justify-center items-center min-h-screen bg-gradient-to-br from-orange-50 via-white to-orange-100">
  <div class="bg-white shadow-2xl rounded-lg p-8 max-w-md w-full border-t-4 border-orange-500">
    <div class="text-center mb-8">
      <img src="/logo.jpeg" alt="SR Automation Logo" class="mx-auto w-20 h-20 mb-4" />
      <h1 class="text-4xl font-bold text-gray-800">Register</h1>
      <p class="text-sm text-gray-600 font-medium">Create your account to get started.</p>
    </div>

    <form on:submit|preventDefault={handleRegister} class="space-y-6">
      <div>
        <label for="email" class="block text-sm font-medium text-gray-700">Email Address</label>
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
        <label for="password" class="block text-sm font-medium text-gray-700">Password</label>
        <input
          id="password"
          type="password"
          bind:value={password}
          required
          class="mt-1 block w-full px-4 py-3 border border-gray-300 rounded-lg shadow-sm focus:outline-none focus:ring-1 focus:ring-orange-500 focus:border-orange-500 placeholder-gray-400"
          placeholder="Enter your password"
        />
      </div>
      <div>
        <label for="confirmPassword" class="block text-sm font-medium text-gray-700">Confirm Password</label>
        <input
          id="confirmPassword"
          type="password"
          bind:value={confirmPassword}
          required
          class="mt-1 block w-full px-4 py-3 border border-gray-300 rounded-lg shadow-sm focus:outline-none focus:ring-1 focus:ring-orange-500 focus:border-orange-500 placeholder-gray-400"
          placeholder="Confirm your password"
        />
      </div>
      <button
        type="submit"
        class="w-full bg-orange-600 text-white font-semibold py-3 px-4 rounded-lg shadow-lg hover:bg-orange-700 focus:outline-none focus:ring focus:ring-orange-300 transition"
        disabled={loading}
      >
        {loading ? 'Registering...' : 'Register'}
      </button>
      {#if showError}
        <div class="mt-4 text-sm text-red-500">{errorMessage}</div>
      {/if}
    </form>

    <div class="mt-8 text-center text-sm text-gray-500">
      <p>Already have an account? <a href="/login" class="text-orange-600 font-medium hover:underline">Login</a></p>
    </div>
  </div>
</div>
