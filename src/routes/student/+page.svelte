<script>
    import { writable } from 'svelte/store';
    import { Navbar, NavBrand, NavLi, NavUl, NavHamburger, Avatar, Dropdown, DropdownItem, DropdownHeader, DropdownDivider } from 'flowbite-svelte';
    import { Table, TableBody, TableBodyCell, TableBodyRow, TableHead, TableHeadCell } from 'flowbite-svelte';

    // State to manage dark mode
    const isDarkMode = writable(true);

    let items = [
        { id: 1, subject: 'Sub 1', teacher: 'Teacher 1', start_t: "9 am" , end_t:"10 am" },
        { id: 2, subject: 'Sub 2', teacher: 'Teacher 2', start_t: "10 am" , end_t:"11 am" },
        { id: 3, subject: 'Sub 3', teacher: 'Teacher 3', start_t: "11:20 am" , end_t:"12:30 pm" },
        { id: 4, subject: 'Sub 4', teacher: 'Teacher 3', start_t: "1:20 pm" , end_t:"2:15 pm" }
    ]

    const sortKey = writable('id'); // default sort key
    const sortDirection = writable(1); // default sort direction (ascending)
    const sortItems = writable(items.slice()); // make a copy of the items array

    // Define a function to sort the items
    function sortTable(key) {
        if ($sortKey === key) {
            sortDirection.update(val => -val);
        } else {
            sortKey.set(key);
            sortDirection.set(1);
        }
    }

    $: {
        const key = $sortKey;
        const direction = $sortDirection;
        const sorted = [...items].sort((a, b) => {
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

    // Toggle dark mode
    function toggleDarkMode() {
        isDarkMode.update(value => !value);
    }
</script>

<!-- Bind dark mode class -->
<div class:dark={$isDarkMode}>
    <!-- Navbar with dropdown menu and avatar -->
    <Navbar>
        <NavBrand href="/">
            <span class="self-center whitespace-nowrap text-xl font-semibold dark:text-white">Student Page</span>
        </NavBrand>
        <div class="flex items-center md:order-2">
            <Avatar id="avatar-menu" src="/images/profile-picture-3.webp" />
            <!-- <button class="ml-4 p-2 text-sm bg-gray-200 dark:bg-gray-800 rounded" on:click={toggleDarkMode}>
                {#if $isDarkMode} Light Mode 
                {:else} Dark Mode {/if}
            </button> -->
            <NavHamburger class1="w-full md:flex md:w-auto md:order-1" />
        </div>
        <Dropdown placement="bottom" triggeredBy="#avatar-menu">
            <DropdownHeader>
                <span class="block text-sm">Bonnie Green</span>
                <span class="block truncate text-sm font-medium">name@flowbite.com</span>
            </DropdownHeader>
            <DropdownItem>Dashboard</DropdownItem>
            <DropdownItem>Settings</DropdownItem>
            <DropdownDivider />
            <DropdownItem>Sign out</DropdownItem>
        </Dropdown>
        <NavUl>
            <NavLi href="/" active={true}>Home</NavLi>
        </NavUl>
    </Navbar>

    <!-- Class schedules -->
    <div>
        <Table hoverable={true}>
            <caption class="p-5 text-lg font-semibold text-left text-gray-900 bg-white dark:text-white dark:bg-gray-800">
                Class schedules
                <p class="mt-1 text-sm font-normal text-gray-500 dark:text-gray-400">Here is the list of the classes scheduled today and their timings</p>
            </caption>
            <TableHead>
                <TableHeadCell on:click={() => sortTable('id')}>No</TableHeadCell>
                <TableHeadCell on:click={() => sortTable('subject')}>Subject</TableHeadCell>
                <TableHeadCell on:click={() => sortTable('teacher')}>Teacher</TableHeadCell>
                <TableHeadCell on:click={() => sortTable('start_t')}>Start Time</TableHeadCell>
                <TableHeadCell on:click={() => sortTable('end_t')}>End Time</TableHeadCell>
            </TableHead>
            <TableBody tableBodyClass="divide-y">
                {#each $sortItems as item}
                    <TableBodyRow>
                        <TableBodyCell>{item.id}</TableBodyCell>
                        <TableBodyCell>{item.subject}</TableBodyCell>
                        <TableBodyCell>{item.teacher}</TableBodyCell>
                        <TableBodyCell>{item.start_t}</TableBodyCell>
                        <TableBodyCell>{item.end_t}</TableBodyCell>
                    </TableBodyRow>
                {/each}
            </TableBody>
        </Table>
    </div>


    <!-- progress & grade -->
    <div class=" dark:text-white dark:bg-gray-900 p-5">
        <div>
            <h3 class="text-lg font-semibold text-left">Progress & Grade</h3>
            <p class="mt-1 text-sm font-normal text-gray-500 dark:text-gray-400">Here is your progress in each subject and your grades</p>
        </div>

    </div>
</div>

<style>
    /* This ensures the dark mode is applied correctly */
    :global(body) {
        transition: background-color 0.3s;
    }
</style>
