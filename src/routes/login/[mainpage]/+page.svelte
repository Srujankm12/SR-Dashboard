<script>
  import { goto } from "$app/navigation";
  import { page } from "$app/stores";
  export let data;
  
  let isLoading = false;

  const navigate = (url) => {
    isLoading = true;
    setTimeout(() => {
      goto(url);
      isLoading = false;
    }, 1000); // Simulate a delay for demonstration
  };
</script>

<div class="min-h-screen flex flex-col bg-gradient-to-br from-orange-50 via-white to-orange-100">

  <header class="bg-gradient-to-r from-orange-600 to-orange-500 text-white shadow-lg">
    <div class="container mx-auto px-6 py-4 flex items-center justify-between">
      <!-- Logo and Branding -->
      <div class="flex items-center space-x-4">
        <img src="/logo.jpeg" alt="SRA BAO Logo" class="w-16 h-16 rounded-full border-4 border-white shadow-md" />
        <div>
          <h1 class="text-3xl font-extrabold leading-tight tracking-wide">SRA BAO</h1>
          <p class="text-sm font-medium opacity-90">Daily Reporting System</p>
        </div>
      </div>
  
     
    </div>
  </header>
  
  {#if isLoading}
    <div class="fixed inset-0 flex items-center justify-center z-40 bg-white bg-opacity-75">
      <div class="w-16 h-16 border-6 border-t-8 border-orange-500 border-solid rounded-full animate-spin"></div>
    </div>
  {:else}
    <main class="flex-grow container mx-auto px-6 py-8">
      <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-8 mt-6">
        <div class="rounded-lg p-6 bg-white flex flex-col items-center justify-between h-72 border-t-4 border-orange-500 shadow-lg hover:shadow-xl hover:scale-105 transition-transform duration-200">
          <h2 class="text-lg font-bold text-gray-800 text-center mb-2">Technical Team Report</h2>
          <p class="text-gray-600 text-sm text-center mb-4">Monitor daily technical progress and team updates.</p>
          <button 
            class="text-sm py-2 px-4 rounded-md bg-orange-500 text-white font-semibold hover:bg-orange-600 transition duration-200"
            on:click={() => navigate('/login/' + $page.params.mainpage + "/dashboard")}>
            Fill Report
          </button>
        </div>
        <div class="rounded-lg p-6 bg-white flex flex-col items-center justify-between min-w-[250px] max-w-[350px] h-auto border-t-4 border-orange-500 shadow-lg hover:shadow-xl hover:scale-105 transition-transform duration-200">
          <h2 class="text-lg font-bold text-gray-800 text-center mb-2">Sales Team Report</h2>
          <p class="text-gray-600 text-sm text-center mb-4">View client engagement and daily sales metrics insights.</p>
          <button 
            class="text-sm py-2 px-4 rounded-md bg-orange-500 text-white font-semibold hover:bg-orange-600 transition duration-200"
            on:click={() => navigate('/login/' + $page.params.mainpage + "/sales")}>
            Fill Report
          </button>
        </div>
      </div>
    </main>

    <footer class="bg-orange-600 text-white py-4 shadow-inner">
      <div class="container mx-auto px-4 text-center text-sm">
        <p>&copy; 2024 SRA BAO. All Rights Reserved.</p>
      </div>
    </footer>
  {/if}
</div>
