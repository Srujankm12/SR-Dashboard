<script>
  import { page } from '$app/stores';

  let reportDate = "";
  let employeeName = "";
  let premises = "Office";
  let siteLocation = "";
  let clientName = "";
  let workScope = "";
  let workDetails = "";
  let jointMeetings = "";
  let supportNeeded = "";
  let workStatus = "";
  let workPriority = "";
  let actionPlan = "";
  let reportSummary = "";
  let taskType = "";
  let closingTime = "";
  let contactPersonName = "";
  let customerEmail = "";
  let typeOfWork = "";
  let doc1 = null;
  let doc2 = null;

  let successMessage = "";
  let errorMessage = "";

  const resetForm = () => {
    reportDate = "";
    employeeName = "";
    premises = "Office";
    siteLocation = "";
    clientName = "";
    workScope = "";
    workDetails = "";
    jointMeetings = "";
    supportNeeded = "";
    workStatus = "";
    workPriority = "";
    actionPlan = "";
    reportSummary = "";
    taskType = "";
    closingTime = "";
    contactPersonName = "";
    customerEmail = "";
    typeOfWork = "";
    doc1 = null;
    doc2 = null;
  };

  const submit = async () => {
    if (!doc1 || !doc2) {
      errorMessage = "Please upload both required documents.";
      setTimeout(() => (errorMessage = ""), 3000);
      return;
    }

    const payload = {
      user_id: $page.params.mainpage,
      report_date: reportDate,
      employee_name: employeeName,
      premises,
      site_location: siteLocation,
      client_name: clientName,
      scope_of_work: workScope,
      work_details: workDetails,
      joint_visits: jointMeetings,
      support_needed: supportNeeded,
      status_of_work: workStatus,
      priority_of_work: workPriority,
      next_action_plan: actionPlan,
      result: reportSummary,
      type_of_work: typeOfWork,
      closing_time: closingTime,
      contact_person_name: contactPersonName,
      contact_emailid: customerEmail,
    };

    const formData = new FormData();
    formData.append("file1", doc1);
    formData.append("file2", doc2);
    formData.append("json_data", JSON.stringify(payload));

    try {
      const response = await fetch("http://localhost:8000/submit", {
        method: "POST",
        body: formData,
      });

      if (response.ok) {
        resetForm(); // Clear all form fields
        successMessage = "Report submitted successfully!";
        setTimeout(() => (successMessage = ""), 6000);
        location.reload(); // Refresh the page
      } else {
        const errorText = await response.text();
        errorMessage = `Error submitting the report: ${errorText}`;
        setTimeout(() => (errorMessage = ""), 3000);
      }
    } catch (err) {
      errorMessage = "Failed to communicate with the server. Please try again later.";
      setTimeout(() => (errorMessage = ""), 3000);
    }
  };
</script>

<div class="relative">
  {#if successMessage}
    <div class="fixed bottom-4 right-4 bg-orange-500 text-white px-4 py-2 rounded shadow-lg">
      {successMessage}
    </div>
  {/if}

  {#if errorMessage}
    <div class="fixed bottom-4 right-4 bg-white text-orange-500 border border-orange-500 px-4 py-2 rounded shadow-lg">
      {errorMessage}
    </div>
  {/if}

  <!-- Form Contents -->
</div>




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
            <!-- svelte-ignore a11y_label_has_associated_control -->
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
            <textarea id="support-needed" bind:value={supportNeeded} rows="3" placeholder="Specify any support required" class="w-full p-3 border border-gray-300 rounded-md focus:ring-1 focus:ring-orange-600 focus:border-orange-600 focus:outline-none transition-colors"></textarea>
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
      </section>

      <!-- Action Plan and Summary -->
      <section class="mb-8">
        <div class="grid gap-6">
          <div>
            <label for="action-plan" class="block text-gray-700">Next Action Plan <span class="text-red-500">*</span></label>
            <textarea
              id="action-plan"
              bind:value={actionPlan}
              rows="3"
              placeholder="Describe the next action plan"
              class="w-full p-3 border border-gray-300 rounded-md focus:ring-1 focus:ring-orange-600 focus:border-orange-600 focus:outline-none transition-colors"></textarea>
          </div>
          <div>
            <label for="report-summary" class="block text-gray-700">Result</label>
            <textarea
              id="report-summary"
              bind:value={reportSummary}
              rows="3"
              placeholder="Provide the result or summary"
              class="w-full p-3 border border-gray-300 rounded-md focus:ring-1 focus:ring-orange-600 focus:border-orange-600 focus:outline-none transition-colors"></textarea>
          </div>
          <div>
            <label for="closing-time" class="block text-gray-700">Closing Time</label>
            <input
              id="closing-time"
              type="time"
              bind:value={closingTime}
              class="w-full p-3 border border-gray-300 rounded-md focus:ring-1 focus:ring-orange-600 focus:border-orange-600 focus:outline-none transition-colors" />
          </div>
          <div>
            <label for="type-of-work" class="block text-gray-700">Type of Work</label>
            <input
              id="type-of-work"
              type="text"
              bind:value={typeOfWork}
              placeholder="Enter type of work (e.g., technical, administrative)"
              class="w-full p-3 border border-gray-300 rounded-md focus:ring-1 focus:ring-orange-600 focus:border-orange-600 focus:outline-none transition-colors" />
          </div>
        </div>
      </section>
      

      <!-- Additional Information -->
      <section class="mb-8">
        <h3 class="font-semibold text-lg mb-4 text-gray-800">Additional Information</h3>
        <div class="grid gap-6">
          <div>
            <label for="contact-person" class="block text-gray-700">Contact Person Name <span class="text-red-500">*</span></label>
            <input id="contact-person" type="text" bind:value={contactPersonName} placeholder="Enter the contact person's name" class="w-full p-3 border border-gray-300 rounded-md focus:ring-1 focus:ring-orange-600 focus:border-orange-600 focus:outline-none transition-colors" />
          </div>
          <div>
            <label for="customer-email" class="block text-gray-700">Customer Email ID <span class="text-red-500">*</span></label>
            <input id="customer-email" type="email" bind:value={customerEmail} placeholder="Enter the customer's email ID" class="w-full p-3 border border-gray-300 rounded-md focus:ring-1 focus:ring-orange-600 focus:border-orange-600 focus:outline-none transition-colors" />
          </div>
          <div>
            <label for="doc1" class="block text-gray-700">Document 1 <span class="text-red-500">*</span></label>
            <input id="doc1" type="file" on:change={(e) => (doc1 = e.target.files[0])} class="w-full p-3 border border-gray-300 rounded-md focus:ring-1 focus:ring-orange-600 focus:border-orange-600 focus:outline-none transition-colors" />
          </div>
          <div>
            <label for="doc2" class="block text-gray-700">Document 2 <span class="text-red-500">*</span></label>
            <input id="doc2" type="file" on:change={(e) => (doc2 = e.target.files[0])} class="w-full p-3 border border-gray-300 rounded-md focus:ring-1 focus:ring-orange-600 focus:border-orange-600 focus:outline-none transition-colors" />
          </div>
        </div>
      </section>

      <!-- Submit Button -->
      <div class="flex justify-end mt-8">
        <button on:click={submit} class="bg-orange-600 text-white px-6 py-3 rounded-md font-semibold hover:bg-orange-700 transition-colors"
    
        >
        
          Submit Report
        </button>
      </div>
    </div>
  </main>

  <!-- Footer -->
  <footer class="bg-orange-600 text-white py-4 shadow-inner">
    <div class="container mx-auto px-4 text-center text-sm">
      <p>&copy; 2024 SRA BAO. All Rights Reserved.</p>
    </div>
  </footer>
</div>

