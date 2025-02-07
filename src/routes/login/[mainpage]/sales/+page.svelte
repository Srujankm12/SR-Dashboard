<script>
    function toggleInFields() {
        document.getElementById('inFields').classList.toggle('hidden');
    }

    function submitInForm() {
        let checkinStatus = document.getElementById('checkinStatus');
        let timestamp = new Date().toLocaleString();
        
        checkinStatus.innerHTML = `<p class='text-green-500 font-bold'>Checked in successfully at ${timestamp}</p>`;
        document.getElementById('inFields').classList.add('hidden');
        document.getElementById('checkinButton').classList.remove('hidden');
    }

    function toggleCheckinFields() {
        document.getElementById('checkinFields').classList.toggle('hidden');
    }

    function submitCheckinForm() {
        let checkinStatus = document.getElementById('checkinStatus');
        let timestamp = new Date().toLocaleString();
        
        checkinStatus.innerHTML += `<p class='text-blue-500 font-bold'>Checked in at ${timestamp}</p>`;
        document.getElementById('checkinFields').classList.add('hidden');
        document.getElementById('checkoutButton').classList.remove('hidden');
    }

    function toggleCheckoutFields() {
        document.getElementById('checkoutFields').classList.toggle('hidden');
    }

    function submitCheckoutForm() {
        let checkinStatus = document.getElementById('checkinStatus');
        let timestamp = new Date().toLocaleString();
        
        checkinStatus.innerHTML += `<p class='text-red-500 font-bold'>Checked out successfully at ${timestamp}</p>`;
        document.getElementById('checkoutFields').classList.add('hidden');
    }
</script>

<div class="min-h-screen flex flex-col bg-white text-gray-900">
    <header class="bg-black text-white shadow-md py-3 rounded-b-sm">
        <div class="container mx-auto px-6 flex items-center justify-between">
            <h1 class="text-xl font-bold">SRA BAO - Daily Reporting System</h1>
        </div>
    </header>
    
    <main class="flex-grow container mx-auto p-8">
        <div class="bg-white shadow-2xl rounded-lg p-8 border-t-4 border-black">
            <h2 class="text-2xl font-semibold mb-6 text-black">Sales Team Daily Report Form</h2>
            
            <div id="checkinStatus" class="mb-4"></div>
            
            <button onclick={toggleInFields} class="bg-green-500 text-white font-bold py-2 px-4 rounded shadow-md">
                IN
            </button>
            
            <div id="inFields" class="hidden mt-6">
                <label class="block mb-2 text-sm font-medium text-gray-700">Work *</label>
                <select class="w-full border border-gray-300 rounded px-3 py-2">
                    <option value="">Select Work</option>
                    <option value="Office">Office</option>
                    <option value="Site">Site</option>
                </select>
                
                <label class="block mt-4 mb-2 text-sm font-medium text-gray-700">Today's Work Plan *</label>
                <textarea class="w-full border border-gray-300 rounded px-3 py-2" rows="4" placeholder="Enter work plan..."></textarea>
                
                <button onclick={submitInForm} class="mt-4 bg-blue-500 text-white font-bold py-2 px-4 rounded shadow-md">
                    Submit
                </button>
            </div>
            
            <button id="checkinButton" onclick={toggleCheckinFields} class="mt-4 bg-blue-500 text-white font-bold py-2 px-4 rounded shadow-md hidden">
                Check-in
            </button>
            
            <div id="checkinFields" class="hidden mt-6">
                <label class="block mb-2 text-sm font-medium text-gray-700">Company Name *</label>
                <select class="w-full border border-gray-300 rounded px-3 py-2">
                    <option value="">Select Company</option>
                    <option value="Company A">Company A</option>
                    <option value="Company B">Company B</option>
                </select>
                
                <label class="block mt-4 mb-2 text-sm font-medium text-gray-700">Purpose of Visit/Meeting/Call *</label>
                <textarea class="w-full border border-gray-300 rounded px-3 py-2" rows="4" placeholder="Enter purpose..."></textarea>
                
                <button onclick={submitCheckinForm} class="mt-4 bg-blue-500 text-white font-bold py-2 px-4 rounded shadow-md">
                    Submit
                </button>
            </div>

            <button id="checkoutButton" onclick={toggleCheckoutFields} class="mt-4 bg-red-500 text-white font-bold py-2 px-4 rounded shadow-md hidden">
                Check-out
            </button>
            
            <div id="checkoutFields" class="hidden mt-6">
                <label class="block mb-2 text-sm font-medium text-gray-700">Engineer Name *</label>
                <input type="text" class="w-full border border-gray-300 rounded px-3 py-2">
                
                <label class="block mt-4 mb-2 text-sm font-medium text-gray-700">Company Name *</label>
                <input type="text" class="w-full border border-gray-300 rounded px-3 py-2">
                
                <label class="block mt-4 mb-2 text-sm font-medium text-gray-700">New/Existing *</label>
                <select class="w-full border border-gray-300 rounded px-3 py-2">
                    <option value="">Select</option>
                    <option value="New">New</option>
                    <option value="Existing">Existing</option>
                </select>
                
                <label class="block mt-4 mb-2 text-sm font-medium text-gray-700">Visit On *</label>
                <input type="date" class="w-full border border-gray-300 rounded px-3 py-2">
                
                <label class="block mt-4 mb-2 text-sm font-medium text-gray-700">Company Sales Stage *</label>
                <input type="text" class="w-full border border-gray-300 rounded px-3 py-2">
                
                <label class="block mt-4 mb-2 text-sm font-medium text-gray-700">Next Action Plan *</label>
                <textarea class="w-full border border-gray-300 rounded px-3 py-2"></textarea>
                
                <label class="block mt-4 mb-2 text-sm font-medium text-gray-700">Challenges *</label>
                <textarea class="w-full border border-gray-300 rounded px-3 py-2"></textarea>
                
                <label class="block mt-4 mb-2 text-sm font-medium text-gray-700">Visit Rating *</label>
                <input type="number" min="1" max="5" class="w-full border border-gray-300 rounded px-3 py-2">
                
                <label class="block mt-4 mb-2 text-sm font-medium text-gray-700">Result of the Visit *</label>
                <textarea class="w-full border border-gray-300 rounded px-3 py-2"></textarea>
                
                <button onclick={submitCheckoutForm} class="mt-4 bg-red-500 text-white font-bold py-2 px-4 rounded shadow-md">
                    Submit
                </button>
            </div>
        </div>
    </main>
</div>
