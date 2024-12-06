<script>
  let reportDate = '';
  let employeeName = '';
  let premises = 'Office';
  let siteLocation = '';
  let clientName = '';
  let workScope = '';
  let workDetails = '';
  let jointMeetings = '';
  let supportNeeded = '';
  let workStatus = '';
  let workPriority = '';
  let actionPlan = '';
  let reportSummary = '';
  let taskType = '';
  let closingTime = '';
</script>

<div class="min-h-screen flex flex-col bg-gradient-to-br from-orange-50 via-white to-orange-100">
  <!-- Header -->
  <header class="bg-orange-600 text-white shadow-lg">
    <div class="container mx-auto px-4 py-4 flex justify-between items-center">
      <div class="flex items-center space-x-4">
        <img src="/logo.jpeg" alt="SRA BAO Logo" class="w-10 h-10 rounded-full" />
        <h1 class="text-2xl font-bold">SRA BAO Daily Report</h1>
      </div>
      <nav class="space-x-4">
        <a href="/" class="hover:underline">Home</a>
        <a href="/team" class="hover:underline">Team</a>
        <a href="/contact" class="hover:underline">Contact</a>
      </nav>
    </div>
  </header>

  <!-- Main Content -->
  <main class="flex-grow container mx-auto p-8">
    <div class="bg-white shadow-2xl rounded-lg p-8 border-t-4 border-orange-500">
      <h2 class="text-2xl font-semibold mb-6 text-orange-600">Technical Team Daily Report Form</h2>

      <!-- Basic Information -->
      <section class="mb-8">
        <h3 class="font-semibold text-lg mb-4 text-gray-800">Basic Information</h3>
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          <div>
            <label for="report-date" class="block text-gray-700">Report Date <span class="text-red-500">*</span></label>
            <input id="report-date" type="date" bind:value={reportDate} class="w-full p-3 border border-gray-300 rounded-md focus:ring-1 focus:ring-orange-600 focus:border-orange-600 focus:outline-none transition-colors" />
          </div>
          <div>
            <label for="employee-name" class="block text-gray-700">Employee Name <span class="text-red-500">*</span></label>
            <input id="employee-name" type="text" bind:value={employeeName} placeholder="Enter employee name" class="w-full p-3 border border-gray-300 rounded-md focus:ring-1 focus:ring-orange-600 focus:border-orange-600 focus:outline-none transition-colors" />
          </div>
        </div>
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mt-6">
          <div>
            <label class="block text-gray-700">Premises <span class="text-red-500">*</span></label>
            <div class="flex items-center space-x-4 mt-2">
              <label><input type="radio" bind:group={premises} value="Office" class="mr-2" /> Office</label>
              <label><input type="radio" bind:group={premises} value="Site" class="mr-2" /> Site</label>
            </div>
          </div>
          <div>
            <label for="site-location" class="block text-gray-700">Site Location</label>
            <input id="site-location" type="text" bind:value={siteLocation} placeholder="Enter site location" class="w-full p-3 border border-gray-300 rounded-md focus:ring-1 focus:ring-orange-600 focus:border-orange-600 focus:outline-none transition-colors" />
          </div>
        </div>
      </section>

      <!-- Work Details -->
      <section class="mb-8">
        <h3 class="font-semibold text-lg mb-4 text-gray-800">Work Details</h3>
        <div class="grid gap-6">
          <div>
            <label for="client-name" class="block text-gray-700">Client Name <span class="text-red-500">*</span></label>
            <input id="client-name" type="text" bind:value={clientName} placeholder="Enter client or work name" class="w-full p-3 border border-gray-300 rounded-md focus:ring-1 focus:ring-orange-600 focus:border-orange-600 focus:outline-none transition-colors" />
          </div>
          <div>
            <label for="work-scope" class="block text-gray-700">Scope of Work <span class="text-red-500">*</span></label>
            <textarea id="work-scope" bind:value={workScope} rows="3" placeholder="Define the scope of work" class="w-full p-3 border border-gray-300 rounded-md focus:ring-1 focus:ring-orange-600 focus:border-orange-600 focus:outline-none transition-colors"></textarea>
          </div>
          <div>
            <label for="work-details" class="block text-gray-700">Work Details <span class="text-red-500">*</span></label>
            <textarea id="work-details" bind:value={workDetails} rows="3" placeholder="Provide details of the work" class="w-full p-3 border border-gray-300 rounded-md focus:ring-1 focus:ring-orange-600 focus:border-orange-600 focus:outline-none transition-colors"></textarea>
          </div>
        </div>
      </section>

      <!-- Additional Details -->
      <section class="mb-8">
        <h3 class="font-semibold text-lg mb-4 text-gray-800">Additional Details</h3>
        <div class="grid gap-6">
          <div>
            <label for="joint-meetings" class="block text-gray-700">Joint Visits/Meetings</label>
            <textarea id="joint-meetings" bind:value={jointMeetings} rows="3" placeholder="Enter details of meetings or joint visits" class="w-full p-3 border border-gray-300 rounded-md focus:ring-1 focus:ring-orange-600 focus:border-orange-600 focus:outline-none transition-colors"></textarea>
          </div>
          <div>
            <label for="support-needed" class="block text-gray-700">Support Needed <span class="text-red-500">*</span></label>
            <textarea id="support-needed" bind:value={supportNeeded} rows="3" placeholder="Specify any support required" class="w-full p-3 border border-gray-300 rounded-md focus:ring focus:ring-orange-600 focus:border-orange-600 focus:outline-none transition-colors"></textarea>
          </div>
          <div>
            <label for="work-status" class="block text-gray-700">Status of Work <span class="text-red-500">*</span></label>
            <select id="work-status" bind:value={workStatus} class="w-full p-3 border border-gray-300 rounded-md focus:ring-1 focus:ring-orange-600 focus:border-orange-600 focus:outline-none transition-colors">
              <option value="">Select status</option>
              <option value="Pending">Pending</option>
              <option value="Completed">Completed</option>
              <option value="In Progress">In Progress</option>
            </select>
          </div>
          <div>
            <label for="work-priority" class="block text-gray-700">Priority of Work <span class="text-red-500">*</span></label>
            <select id="work-priority" bind:value={workPriority} class="w-full p-3 border border-gray-300 rounded-md focus:ring-1 focus:ring-orange-600 focus:border-orange-600 focus:outline-none transition-colors">
              <option value="">Select priority</option>
              <option value="High">High</option>
              <option value="Medium">Medium</option>
              <option value="Low">Low</option>
            </select>
          </div>
        </div>
        <!-- Additional Fields -->
<div class="grid gap-6 mt-6">
  <div>
    <label for="action-plan" class="block text-gray-700">Next Action Plan <span class="text-red-500">*</span></label>
    <textarea
      id="action-plan"
      bind:value={actionPlan}
      rows="3"
      placeholder="Describe the next action plan"
      class="w-full p-3 border border-gray-300 rounded-md focus:ring-1 focus:ring-orange-600 focus:border-orange-600 focus:outline-none transition-colors"
    ></textarea>
  </div>
  <div>
    <label for="report-summary" class="block text-gray-700">Result</label>
    <textarea
      id="report-summary"
      bind:value={reportSummary}
      rows="3"
      placeholder="Provide the result or summary"
      class="w-full p-3 border border-gray-300 rounded-md focus:ring-1 focus:ring-orange-600 focus:border-orange-600 focus:outline-none transition-colors"
    ></textarea>
  </div>
  <div>
    <label class="block text-gray-700">Type of Work <span class="text-red-500">*</span></label>
    <div class="flex items-center space-x-4 mt-2">
      <label><input type="radio" bind:group={taskType} value="Chargeable" class="mr-2" /> Chargeable</label>
      <label><input type="radio" bind:group={taskType} value="Non-Chargeable" class="mr-2" /> Non-Chargeable</label>
      <label><input type="radio" bind:group={taskType} value="Projects and Services" class="mr-2" /> Projects and Services</label>
      <label class="flex items-center">
        <input type="radio" value="Other" class="mr-2" /> Other:
        <input type="text"  class="ml-2 p-2 border border-gray-300 rounded-md focus:ring-1 focus:ring-orange-600 focus:border-orange-600 focus:outline-none" />
      </label>
    </div>
  </div>
  <div>
    <label for="closing-time" class="block text-gray-700">Closing Time <span class="text-red-500">*</span></label>
    <input
      id="closing-time"
      type="time"
      bind:value={closingTime}
      class="w-full p-3 border border-gray-300 rounded-md focus:ring-1 focus:ring-orange-600 focus:border-orange-600 focus:outline-none transition-colors"
    />
  </div>
</div>

      </section>

      <!-- Submit Button -->
      <div class="text-center">
        <button class="bg-orange-600 text-white py-3 px-8 rounded-md transition-colors focus:outline-none">
          Submit Report
        </button>
      </div>
    </div>
  </main>

  <!-- Footer -->
  <footer class="bg-orange-600 text-white">
    <div class="container mx-auto px-4 py-4 text-center">
      <p>&copy; 2024 SRA BAO. All Rights Reserved.</p>
    </div>
  </footer>
</div>
