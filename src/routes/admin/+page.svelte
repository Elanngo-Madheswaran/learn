<script>
    import { Sidebar, SidebarWrapper,SidebarItem, SidebarGroup } from 'flowbite-svelte';
    import { Card } from 'flowbite-svelte';
    import { Navbar, NavBrand, NavLi, NavUl, NavHamburger } from 'flowbite-svelte';
    import { Table, TableBody, TableBodyCell, TableBodyRow, TableHead, TableHeadCell } from 'flowbite-svelte';
    import { writable } from 'svelte/store';
    
    let items = [
        { user: "user 1", role: 'Teacher', last_active: 'Today', status: "Active" },
        { user: "user 2", role: 'Student', last_active: 'Yesterday', status: "Inactive"},
        { user: "user 3", role: 'Admin', last_active: 'Today', status: "Active" },
        { user: "user 4", role: 'Teacher', last_active: 'Today', status: "Active" }
    ];
    
    const sortKey = writable('id'); // default sort key
    const sortDirection = writable(1); // default sort direction (ascending)
    const sortItems = writable(items.slice()); // make a copy of the items array
    
    // Define a function to sort the items
    const sortTable = (key) => {
        // If the same key is clicked, reverse the sort direction
        if ($sortKey === key) {
        sortDirection.update((val) => -val);
        } else {
        sortKey.set(key);
        sortDirection.set(1);
        }
    };
    
    $: {
        const key = $sortKey;
        const direction = $sortDirection;
        const sorted = [...$sortItems].sort((a, b) => {
        const aVal = a[key];
        const bVal = b[key];
        if (aVal < bVal) {
            return -direction;
        } else if (aVal > bVal) {
            return direction;
        }
        return 0;
        });
        sortItems.set(sorted);
    }
    let isDarkMode = true; // Set dark mode as default
    let stats={
        users_num: 10,
        active_users_num: 5
    }

    let isSidebarOpen = false;

        function toggleSidebar() {
            isSidebarOpen = !isSidebarOpen;
        }
</script>

<!-- Bind the dark mode class dynamically -->
<div class:dark={isDarkMode} class="min-h-screen flex bg-white">
    <!-- Sidebar and Links -->
    <div class="hidden lg:block">
        <Sidebar>
            <SidebarWrapper class="h-screen">
                <SidebarGroup>
                    <p class="m-5 text-gray-500 transition duration-75 dark:text-white group-hover:text-gray-900 dark:group-hover:text-white uppercase text-3xl font-bold text-center">Admin Dashboard</p>
                    <SidebarItem label="Dashboard" class="text-gray-500 transition duration-75 dark:text-white group-hover:text-gray-900 dark:group-hover:text-white text-xl">
                    </SidebarItem>
                    <SidebarItem label="Manage users" class="text-gray-500 transition duration-75 dark:text-white group-hover:text-gray-900 dark:group-hover:text-white text-xl">
                    </SidebarItem>
                    <SidebarItem label="Roles & Permissions" class="text-gray-500 transition duration-75 dark:text-white group-hover:text-gray-900 dark:group-hover:text-white text-xl">
                    </SidebarItem>
                    <SidebarItem label="Activity logs" class="text-gray-500 transition duration-75 dark:text-white group-hover:text-gray-900 dark:group-hover:text-white text-xl">
                    </SidebarItem>
                    <SidebarItem label="Settings" class="text-gray-500 transition duration-75 dark:text-white group-hover:text-gray-900 dark:group-hover:text-white text-xl">
                    </SidebarItem>
                    <SidebarItem label="Home" class="text-gray-500 transition duration-75 dark:text-white group-hover:text-gray-900 dark:group-hover:text-white text-xl" href="./">
                    </SidebarItem>
                </SidebarGroup>
            </SidebarWrapper>
        </Sidebar>
    </div>

    
    
    <!-- Content area (can be filled with other content) -->
    <div class="w-full">
        <div class="lg:hidden flex">
            <Navbar >
                <NavBrand href="/" class="justify-center flex ">
                  <span class="self-center whitespace-nowrap text-xl font-semibold dark:text-white uppercase ">Admin Dashboard</span>
                </NavBrand>
                <NavHamburger  />
                <NavUl >
                  <NavLi>Dashboard</NavLi>
                  <NavLi>Manage Users</NavLi>
                  <NavLi>Roles & Permissions</NavLi>
                  <NavLi>Activity logs</NavLi>
                  <NavLi>Settings</NavLi>
                  <NavLi href="./">Home</NavLi>
                </NavUl>
            </Navbar>
        </div>
        <div class="p-5">
            <h1 class="text-slate-700 text-3xl font-bold">Overview</h1>
            <div class="flex justify-center flex-wrap">
                <Card class="m-5 flex text-center bg-white dark:bg-white border-0 shadow-xl xl:w-96">
                    <h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-700">Total Users</h5>
                    <p class="font-normal text-gray-700 dark:text-gray-400 leading-tight">{stats.users_num}</p>
                </Card>
                <Card class="m-5 flex text-center bg-white dark:bg-white border-0 shadow-xl xl:w-96">
                    <h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-700">Active Users</h5>
                    <p class="font-normal text-gray-700 dark:text-gray-400 leading-tight">{stats.active_users_num}</p>
                </Card>
            </div>
        </div>
        <div class="p-5">
            <h1 class="text-slate-700 text-3xl font-bold my-5">User Activity Logs</h1>
            <script>
               
              </script>
              
              <Table shadow>
                <TableHead class="dark:text-white">
                  <TableHeadCell on:click={() => sortTable('user')}>User</TableHeadCell>
                  <TableHeadCell on:click={() => sortTable('role')}>Role</TableHeadCell>
                  <TableHeadCell on:click={() => sortTable('last_active')}>Last Active</TableHeadCell>
                  <TableHeadCell on:click={() => sortTable('status')}>Status</TableHeadCell>
                </TableHead>
                <TableBody tableBodyClass="divide-y">
                  {#each $sortItems as item}
                    <TableBodyRow class="dark:hover:bg-slate-300 dark:bg-white">
                      <TableBodyCell class=" dark:text-black">{item.user}</TableBodyCell>
                      <TableBodyCell class=" dark:text-black">{item.role}</TableBodyCell>
                      <TableBodyCell class=" dark:text-black">{item.last_active}</TableBodyCell>
                      <TableBodyCell class=" dark:text-black">{item.status}</TableBodyCell>
                    </TableBodyRow>
                  {/each}
                </TableBody>
              </Table>
        </div>
    </div>
</div>

<style>
    :global(body) {
        transition: background-color 0.3s, color 0.3s;
    }
</style>
