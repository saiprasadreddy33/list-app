<script>
    export let directory;
    export let selectedDirectory;
    export let onSelect;
    export let onDelete;

    let showInfo = false;
    let isSelected = false;
    let totalSubDirectories = 0;
    let totalFiles = 0;
    //let selectedDirectory = null;

    function calculateTotals(dir) {
        totalSubDirectories = 0;
        totalFiles = 0;

        function countItems(directory) {
            if (Array.isArray(directory.children)) {
                totalSubDirectories += directory.children.filter(
                    (c) => c.type === "directory"
                ).length;
                totalFiles += directory.children.filter(
                    (c) => c.type === "file"
                ).length;

                directory.children.forEach((child) => {
                    if (child.type === "directory") {
                        countItems(child);
                    }
                });
            }
        }

        countItems(dir);
    }

    $: calculateTotals(directory);

    function handleClick() {
        if (directory.type === "file") {
            alert("Files do not have subdirectories to create.");
            return;
        }

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

<div class="directory-container relative">
    <div
        class={`directory p-6 rounded-lg border ${
            selectedDirectory === directory ? 'highlighted' : 'border-gray-200'
        } bg-white shadow-sm`}
    >
        <div class="directory-header flex justify-between items-center mb-4">
            {#if directory.type === "directory"}
                <!-- svelte-ignore a11y-no-static-element-interactions -->
                <!-- svelte-ignore a11y-click-events-have-key-events -->
                <span class="directory-name text-lg font-semibold cursor-pointer text-gray-900" on:click={handleClick}>
                    {directory.name}
                </span>
            {/if}
            {#if directory.type === "file"}
                <i class="fas fa-image text-gray-400"></i>
                <span class="text-lg font-semibold text-gray-900">{directory.name}</span>
            {/if}
            <div class="icon-group p-3 flex items-center space-x-6">
                <button class="delete-icon p-2 text-600 rounded-full" on:click={handleDelete}>
                    <i class="fas fa-trash-alt"></i>
                </button>
                <!-- svelte-ignore a11y-mouse-events-have-key-events -->
                <button class="info-icon" on:mouseover={toggleInfo} on:mouseout={toggleInfo}>
                    <i class="fas fa-info"></i>
                </button>
            </div>
        </div>

        {#if directory.type === "directory"}
            <div class="directory-details text-sm text-gray-600 mt-2">
                <span class="flex items-center mb-2">
                    <i class="fas fa-folder mr-1"></i>
                    {Array.isArray(directory.children) ? directory.children.filter((c) => c.type === "directory").length : 0}
                    <span class="ml-4 flex items-center">
                        <i class="fas fa-file mr-1"></i>
                        {Array.isArray(directory.children) ? directory.children.filter((c) => c.type === "file").length : 0}
                    </span>
                </span>
            </div>
        {/if}
    </div>
</div>

{#if showInfo}
<div class="info-popup">
    <p><strong>Name:</strong> {directory.name}</p>
    <p>
        <strong>Immediate Sub-Directories:</strong>
        {Array.isArray(directory.children)
            ? directory.children.filter((c) => c.type === "directory").length
            : 0}
    </p>
    <p>
        <strong>Immediate Files:</strong>
        {Array.isArray(directory.children)
            ? directory.children.filter((c) => c.type === "file").length
            : 0}
    </p>
    <p><strong>Total Sub-Directories:</strong> {totalSubDirectories}</p>
    <p><strong>Total Files:</strong> {totalFiles}</p>
</div>
{/if}

<style>
    .directory-container {
        position: relative;
        z-index: 10;
    }

    .directory {
        position: relative;
        transition: border-color 0.3s ease, background-color 0.3s ease;
    }

    .highlighted {
        background-color: #f0f3ff;
        border-color: #bccbf0;
        animation: fillAnimation 0.5s forwards;
    }

    @keyframes fillAnimation {
        0% {
            background-color: transparent;
        }
        100% {
            background-color: #f0f3ff;
        }
    }

    @keyframes popupAnimation {
        0% {
            opacity: 0;
            transform: translateY(-20px);
        }
        100% {
            opacity: 1;
            transform: translateY(0);
        }
    }

    .info-icon {
        cursor: pointer;
    }

    .info-popup {
        position: absolute;
        padding: 8px 19px;
        gap: 8px;
        background: #ffffff;
        border: 1px solid rgba(50, 50, 50, 0.1);
        border-radius: 0px 10px 10px 10px;
        box-sizing: border-box;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        animation: popupAnimation 0.3s ease-in-out;
        z-index: 20;
        top: 97px;
        left: 17rem;
        min-width: 12rem;
    }

    .directory:hover .delete-icon {
        display: block;
    }

    .delete-icon {
        display: none;
    }
</style>
