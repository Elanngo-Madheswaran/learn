<script>
    import { Sidebar, SidebarWrapper, SidebarBrand, SidebarItem, SidebarGroup } from 'flowbite-svelte';
    import { ChartPieSolid, GridSolid, MailBoxSolid, UserSolid, ArrowRightToBracketOutline, EditOutline } from 'flowbite-svelte-icons';
    
    let spanClass = 'flex-1 ms-3 whitespace-nowrap';

    let site = {
        name: 'Dashboard',
        href: '/'
    };

    let isDarkMode = true; // Set dark mode as default
</script>

<!-- Bind the dark mode class dynamically -->
<div class:dark={isDarkMode} class="min-h-screen flex">
    <!-- Sidebar and Links -->
    <Sidebar class="h-screen">
        <SidebarWrapper>
            <SidebarGroup>
                <SidebarBrand {site} />
                <SidebarItem label="Student Page" href="./student">
                    <svelte:fragment slot="icon">
                        <ChartPieSolid class="w-6 h-6 text-gray-500 transition duration-75 dark:text-gray-400 group-hover:text-gray-900 dark:group-hover:text-white" />
                    </svelte:fragment>
                </SidebarItem>
                <SidebarItem label="Teacher Page" href="./teacher" {spanClass}>
                    <svelte:fragment slot="icon">
                        <GridSolid class="w-6 h-6 text-gray-500 transition duration-75 dark:text-gray-400 group-hover:text-gray-900 dark:group-hover:text-white" />
                    </svelte:fragment>
                </SidebarItem>
                <SidebarItem label="Admin Page" {spanClass}>
                    <svelte:fragment slot="icon">
                        <MailBoxSolid class="w-6 h-6 text-gray-500 transition duration-75 dark:text-gray-400 group-hover:text-gray-900 dark:group-hover:text-white" />
                    </svelte:fragment>
                </SidebarItem>
                <SidebarItem label="Users">
                    <svelte:fragment slot="icon">
                        <UserSolid class="w-6 h-6 text-gray-500 transition duration-75 dark:text-gray-400 group-hover:text-gray-900 dark:group-hover:text-white" />
                    </svelte:fragment>
                </SidebarItem>
                <SidebarItem label="Sign In">
                    <svelte:fragment slot="icon">
                        <ArrowRightToBracketOutline class="w-6 h-6 text-gray-500 transition duration-75 dark:text-gray-400 group-hover:text-gray-900 dark:group-hover:text-white" />
                    </svelte:fragment>
                </SidebarItem>
                <SidebarItem label="Sign Up">
                    <svelte:fragment slot="icon">
                        <EditOutline class="w-6 h-6 text-gray-500 transition duration-75 dark:text-gray-400 group-hover:text-gray-900 dark:group-hover:text-white" />
                    </svelte:fragment>
                </SidebarItem>
            </SidebarGroup>
        </SidebarWrapper>
    </Sidebar>

    <!-- Content area (can be filled with other content) -->
    <div class="flex-1 p-5 text-white">
        <!-- Your content goes here -->
    </div>
</div>

<style>
    :global(body) {
        transition: background-color 0.3s, color 0.3s;
    }
</style>
