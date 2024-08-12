<script>
export let directory;
export let onAdd;
export let onSelect;
export let onDelete;

let showInfo = false;
let totalSubDirectories = 0;
let totalFiles = 0;

import {
    onMount
} from 'svelte';

onMount(() => {
    function calculateTotals(dir) {
        if (Array.isArray(dir.children)) {
            totalSubDirectories += dir.children.filter(c => c.type === 'directory').length;
            totalFiles += dir.children.filter(c => c.type === 'file').length;

            dir.children.forEach(child => {
                if (child.type === 'directory') {
                    calculateTotals(child);
                }
            });
        }
    }

    calculateTotals(directory);
});

function handleAdd() {
    onAdd(directory);
}

function handleClick() {
    onSelect(directory);
}

function handleDelete() {
    if (onDelete) {
        onDelete(directory);
    }
}

function toggleInfo() {
    showInfo = !showInfo;
}
</script>

<div class="directory p-6 rounded-lg border border-gray-200 bg-white shadow-sm">
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <div class="directory-header flex justify-between items-center mb-4">
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <span class="directory-name text-lg font-semibold cursor-pointer text-gray-900 space-x-6" on:click={handleClick}>
            {directory.name}
        </span>
        <div class="icon-group p-3 flex items-center space-x-6">
            <!-- svelte-ignore a11y-mouse-events-have-key-events -->
            <button on:mouseover={toggleInfo} on:mouseout={toggleInfo}>
                <i class="fas fa-info-circle"></i>
                {#if showInfo}
                <div>
                    <p><strong>Name:</strong> {directory.name}</p>
                    <p><strong>Immediate Sub-Directories:</strong> {Array.isArray(directory.children) ? directory.children.filter(c => c.type === 'directory').length : 0}</p>
                    <p><strong>Immediate Files:</strong> {Array.isArray(directory.children) ? directory.children.filter(c => c.type === 'file').length : 0}</p>
                    <p><strong>Total Sub-Directories:</strong> {totalSubDirectories}</p>
                    <p><strong>Total Files:</strong> {totalFiles}</p>
                </div>
                {/if}
            </button>
            <button class="delete-icon p-2 bg-red-100 text-red-600 rounded-full hover:bg-red-200" on:click={handleDelete}>
                <i class="fas fa-trash-alt"></i>
            </button>
        </div>
    </div>

    <div class="directory-details text-sm text-gray-600 mt-2">
        <span class="flex items-center mb-2">
            <i class="fas fa-folder mr-1"></i> {Array.isArray(directory.children) ? directory.children.filter(c => c.type === 'directory').length : 0}
            <span class="ml-4 flex items-center">
                <i class="fas fa-file mr-1"></i> {Array.isArray(directory.children) ? directory.children.filter(c => c.type === 'file').length : 0}
            </span>
        </span>
    </div>
</div>
