<script>
  import { goto } from "$app/navigation";
  import { page } from "$app/stores";
	import Header from "$lib/header.svelte";
  export let data;
  
  let isLoading = false;

  const navigate = (url) => {
    isLoading = true;
    setTimeout(() => {
      goto(url);
      isLoading = false;
    }, 1000);
  };
</script>

<div class="min-h-screen flex flex-col bg-white text-gray-900">
<Header />
  
  {#if isLoading}
    <div class="fixed inset-0 flex items-center justify-center bg-white bg-opacity-75 z-50">
      <div class="w-12 h-12 border-4 border-t-4 border-gray-700 rounded-full animate-spin"></div>
    </div>
  {:else}
    <main class="flex-grow container mx-auto px-6 py-28">
      <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6">
        <div class="bg-white rounded-lg shadow-lg p-6 flex flex-col items-center justify-between h-64 border-t-4 border-black hover:shadow-xl hover:scale-105 transition-transform duration-200">
          <h2 class="text-lg font-semibold text-center">Technical Team Report</h2>
          <p class="text-gray-600 text-sm text-center">Monitor daily technical progress and team updates.</p>
          <button 
            class="text-sm py-2 px-4 rounded bg-black text-white font-semibold transition duration-200"
            on:click={() => navigate('/login/' + $page.params.mainpage + "/dashboard")}
          >
            Fill Report
          </button>
        </div>
        <div class="bg-white rounded-lg shadow-lg p-6 flex flex-col items-center justify-between h-64 border-t-4 border-black hover:shadow-xl hover:scale-105 transition-transform duration-200">
          <h2 class="text-lg font-semibold text-center">Sales Team Report</h2>
          <p class="text-gray-600 text-sm text-center">View client engagement and daily sales metrics insights.</p>
          <button 
            class="text-sm py-2 px-4 rounded bg-black text-white font-semibold transition duration-200"
            on:click={() => navigate('/login/' + $page.params.mainpage + "/sales")}
          >
            Fill Report
          </button>
        </div>
      </div>
    </main>

  
    
  {/if}
</div>
