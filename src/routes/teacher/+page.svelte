<script>
    import { writable } from 'svelte/store';
    import { Navbar, NavBrand, NavLi, NavUl, NavHamburger, Avatar, Dropdown, DropdownItem, DropdownHeader, DropdownDivider } from 'flowbite-svelte';
    import { Table, TableBody, TableBodyCell, TableBodyRow, TableHead, TableHeadCell } from 'flowbite-svelte';
    import { Progressbar } from 'flowbite-svelte';
    import { Card, Button, Toggle } from 'flowbite-svelte';
    import { ArrowRightOutline } from 'flowbite-svelte-icons';
    let vCard = false;

    // State to manage dark mode
    const isDarkMode = writable(true);

    let items = [
        { id: 1, class: 'class 1', start_t: "9 am", end_t: "10 am" },
        { id: 2, class: 'class 2', start_t: "10 am", end_t: "11 am" },
        { id: 3, class: 'class 3', start_t: "11:20 am", end_t: "12:30 pm" },
        { id: 4, class: 'class 4', start_t: "1:20 pm", end_t: "2:15 pm" }
    ];

    let progress_val = [
        { id: 1, student: 'Student 1', progress: 80, grade: 'A' },
        { id: 2, student: 'Student 2', progress: 70, grade: 'B' },
        { id: 3, student: 'Student 3', progress: 90, grade: 'A' },
        { id: 4, student: 'Student 4', progress: 60, grade: 'C' }
    ];

    let courses = [
        {title:"Total & Average", desc:"This is the tool for calculating total and average"},
        {title:"Grade", desc:"This is the tool to assign grades to students"},
        {title:"CGPA", desc:"This is the tool to calculate CGPA"}
    ];

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
<div class:dark={$isDarkMode} class="selection:bg-orange-500 selection:text-white">
    <!-- Navbar with dropdown menu and avatar -->
    <Navbar>
        <NavBrand href="/">
            <span class="self-center whitespace-nowrap text-xl font-semibold dark:text-white">Teacher Page</span>
        </NavBrand>
        <div class="flex items-center md:order-2">
            <Avatar id="avatar-menu" src="/images/profile-picture-3.webp" />
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
    <div class="p-5 dark:bg-gray-900">
        <Table hoverable={true}>
            <caption class="py-5 text-lg font-semibold text-left text-gray-900 dark:text-white">
                Class schedules
                <p class="mt-1 text-sm font-normal text-gray-500 dark:text-gray-400">Here is the list of the classes scheduled today and their timings</p>
            </caption>
            <TableHead>
                <TableHeadCell on:click={() => sortTable('id')}>No</TableHeadCell>
                <TableHeadCell on:click={() => sortTable('class')}>Class</TableHeadCell>
                <TableHeadCell on:click={() => sortTable('start_t')}>Start Time</TableHeadCell>
                <TableHeadCell on:click={() => sortTable('end_t')}>End Time</TableHeadCell>
            </TableHead>
            <TableBody tableBodyClass="divide-y dark:divide-gray-700">
                {#each $sortItems as item}
                    <TableBodyRow class="dark:bg-gray-800">
                        <TableBodyCell>{item.id}</TableBodyCell>
                        <TableBodyCell>{item.class}</TableBodyCell>
                        <TableBodyCell>{item.start_t}</TableBodyCell>
                        <TableBodyCell>{item.end_t}</TableBodyCell>
                    </TableBodyRow>
                {/each}
            </TableBody>
        </Table>
    </div>

    <!-- Student Progress & Grade -->
    <div class="p-5 dark:bg-gray-900 dark:text-white">
        <div>
            <h3 class="text-lg font-semibold text-left">Student Progress & Grades</h3>
            <p class="mt-1 text-sm font-normal text-gray-500 dark:text-gray-400">
                Here is your student's progress and their grades
            </p>
        </div>
        {#each progress_val as item (item.id)}
            <div class="flex items-center justify-between mt-4 dark:bg-gray-800 p-2 rounded" id={`progress-${item.id}`}>
                <div class="flex items-center">
                    <span class="sm:text-2xl sm:mx-5 mx-2 font-semibold text-md">{item.student}</span>
                    <span class="ml-2 sm:text-xl text-sm font-normal text-gray-500 dark:text-gray-400">Grade : {item.grade}</span>
                </div>
                <!-- Assign unique id for each progress bar -->
                <Progressbar progress={item.progress} class="w-1/2 dark:bg-gray-700 h-4" id={`progressbar-${item.id}`} labelInside />
            </div>
        {/each}
    </div>

    <!-- Grading Tools -->
    <div class="p-5 dark:bg-gray-900 dark:text-white">
        <div>
            <h3 class="text-lg font-semibold text-left">Grading Tools</h3>
            <p class="mt-1 text-sm font-normal text-gray-500 dark:text-gray-400">
                Here is your tools for grading
            </p>
        </div>
        <div  class="flex flex-wrap m-5">
        {#each courses as course (course.title)}
            <Card img="/images/image-1.webp" reverse={vCard} class="m-2">
                <h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">{course.title}</h5>
                <p class="mb-3 font-normal text-gray-700 dark:text-gray-400 leading-tight">{course.desc}</p>
                <Button>
                Tool <ArrowRightOutline class="w-6 h-6 ms-2 text-white" />
                </Button>
            </Card>
        {/each}
        </div>
    </div>
</div>

<style>
    /* This ensures the dark mode is applied correctly */
    :global(body) {
        transition: background-color 0.3s, color 0.3s;
    }
</style>
