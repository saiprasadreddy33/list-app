<script>
    import Modal from "./components/Modal.svelte";
    import Directory from "./components/Directory.svelte";

    let root = {
        name: "ROOT",
        type: "directory",
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
            children: itemType === "directory" ? [] : null,
        };

        selectedDirectory.children.push(newItem);
        root = { ...root }; 
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
        const index = panels.findIndex((panel) => panel === directory);

        if (index === -1) {
            const parentIndex = panels.findIndex((panel) =>
                panel.children.includes(directory)
            );

            if (parentIndex !== -1) {
                panels = panels.slice(0, parentIndex + 1);
            }

            panels.push(directory);
        } else {
            panels = panels.slice(0, index + 1);
        }
    }

    function deleteItem(directory) {
        function deleteRecursively(dir, target) {
            dir.children = dir.children.filter((child) => {
                if (child === target) return false;
                if (child.type === "directory") {
                    deleteRecursively(child, target);
                }
                return true;
            });
        }

        if (directory === root) {
            root = { name: "ROOT", type: "directory", children: [] };
            panels = [root];
        } else {
            const parentDirectory = panels.find((panel) =>
                panel.children.includes(directory)
            );

            if (parentDirectory) {
                deleteRecursively(parentDirectory, directory);
                root = { ...root };
                updatePanels(parentDirectory);
            }
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
                <button class="bg-black text-white px-3 py-1 rounded-md hover:bg-slate-200" on:click={() => openModal(panel)}>
                    <i class="fas fa-plus"></i>
                </button>
            </div>
            <div class="p-10 overflow-y-auto w-full max-h-screen">
                {#each panel.children as directory}
                    <Directory
                        directory={directory}
                        selectedDirectory={selectedDirectory}
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
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
    main {
        display: flex;
        gap: 1rem;
    }

    div.relative {
        min-width: 20rem;
    }
</style>
