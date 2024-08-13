<script>
    export let onAdd;
    export let onClose;
    export let itemType = "directory";
    let itemName = "";

    function handleAdd() {
        if (itemName.trim() === "") {
            alert("Name is required");
            return;
        }
        onAdd(itemName, itemType);
        itemName = "";
    }
</script>

<div
    class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-60 z-50"
>
    <div class="bg-white rounded-lg shadow-lg w-80 p-6">
        <h2 class="text-xl font-semibold text-gray-900 mb-4">Add New</h2>
        <div class="mb-4">
            <!-- svelte-ignore a11y-label-has-associated-control -->
            <label class="block text-sm font-medium text-gray-700 mb-2"
                >Type:</label
            >
            <div class="flex items-center space-x-4">
                <label class="flex items-center cursor-pointer">
                    <input
                        type="radio"
                        bind:group={itemType}
                        value="file"
                        class="form-radio text-indigo-600"
                    />
                    <span class="ml-2 text-gray-800">File</span>
                </label>

                <label class="flex items-center cursor-pointer">
                    <input
                        type="radio"
                        bind:group={itemType}
                        value="directory"
                        class="form-radio text-indigo-600"
                    />
                    <span class="ml-2 text-gray-800">Directory</span>
                </label>
            </div>
        </div>
        <div class="mb-4">
            <!-- svelte-ignore a11y-label-has-associated-control -->
            <label class="block text-sm font-medium text-gray-700 mb-2"
                >Name:</label
            >
            <input
                type="text"
                bind:value={itemName}
                class="block w-full border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm p-2"
            />
        </div>
        <div class="flex justify-end space-x-4">
            <button
                on:click={onClose}
                class="px-4 py-2 bg-gray-200 text-gray-700 rounded-md hover:bg-gray-300"
                >Cancel</button
            >

            <button
                on:click={handleAdd}
                class="px-4 py-2 rounded-md"
                class:bg-black={itemName.trim() !== ""}
                class:bg-gray-400={itemName.trim() === ""}
                class:text-white={itemName.trim() !== ""}
                class:text-gray-700={itemName.trim() === ""}
                class:cursor-pointer={itemName.trim() !== ""}
                class:cursor-not-allowed={itemName.trim() === ""}
                disabled={itemName.trim() === ""}
            >
                <i class="fas fa-plus mr-2"></i>Add
            </button>
        </div>
    </div>
</div>

<style>
    .form-radio {
        accent-color: #4f46e5;
    }

    button:disabled {
        background-color: #9ca3af;
        cursor: not-allowed;
    }

    button {
        cursor: pointer;
    }
</style>
