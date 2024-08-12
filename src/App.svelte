<script>
import Modal from './components/Modal.svelte';
import Directory from './components/Directory.svelte';

let root = {
    name: 'ROOT',
    type: 'directory',
    children: [],
};

let showModal = false;
let selectedDirectory = root;
let panels = [root];

function addItem(itemName, itemType) {
    if (!selectedDirectory) return;

    const newItem = {
        name: itemName,
        type: itemType,
        children: itemType === 'directory' ? [] : null,
    };

    selectedDirectory.children.push(newItem);
    root = {
        ...root
    }; // Trigger reactivity
    updatePanels(selectedDirectory);
    closeModal();
}

function openModal(directory) {
    selectedDirectory = directory;
    showModal = true;
}

function closeModal() {
    showModal = false;
}

function updatePanels(directory) {
    const index = panels.indexOf(directory);
    if (index !== -1) {
        panels = panels.slice(0, index + 1); 
    } else {
        panels.push(directory);
    }
}

function deleteItem(directory) {
    const parentDirectory = panels.find(panel =>
        panel.children.includes(directory)
    );

    if (parentDirectory) {
        parentDirectory.children = parentDirectory.children.filter(
            child => child !== directory
        );
        root = {
            ...root
        };
        updatePanels(selectedDirectory);
    }
}

function handleDirectoryClick(directory) {
    selectedDirectory = directory;
    updatePanels(directory);
}
</script>

<main class="min-h-screen w-full flex overflow-x-auto bg-gray-50">
    {#each panels as panel, index}
    <div class="relative w-80 bg-white border border-gray-200 rounded-lg shadow-lg mr-4 mb-4">
        <div class="p-4 flex justify-between items-center bg-gray-50 border-b border-gray-200">
            <span class="font-semibold text-gray-900">{panel.name}</span>
            <button
                class="bg-black text-white px-3 py-1 rounded-md hover:bg-slate-200"
                on:click={() => openModal(panel)}
                >
                <i class="fas fa-plus"></i>
            </button>
        </div>
        <div class="p-10 overflow-y-auto w-full max-h-screen">
            {#each panel.children as directory}
            <Directory
                directory={directory || { name: '', children: [] }}
                onAdd={openModal}
                onSelect={handleDirectoryClick}
                onDelete={deleteItem}
                />

                {/each}
                </div>
                </div>
                {/each}

                {#if showModal}
                <Modal onAdd={addItem} onClose={closeModal} />
                {/if}
                </main>

<style>
main {
    display: flex;
    gap: 1rem;
}

div.relative {
    min-width: 20rem;
}

@tailwind base;
@tailwind components;
@tailwind utilities;
</style>
