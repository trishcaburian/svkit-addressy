<nav class="flex items-center justify-between flex-wrap bg-gray-800 p-6 text-white">
    <span class="font-bold text-xl">Address Book</span>
    <div class="w-full block flex-grow px-4 sm:flex sm:items-center sm:w-auto">
        <div class="sidebar" on:click={() => (window.location.href = "/")}> Home </div>
        <div class="sidebar" on:click={() => (window.location.href = "/add_contact")}> Add </div>
    </div>
</nav>

<slot/>