<template>
    <div class="sidebar">
        <header class="section__header">
            <h2>Collections</h2>
        </header>

        <div class="section__inner">
            <nav
                class="collections__nav"
                v-if="collections"
            >
                <ul class="collections__list">
                    <li
                        class="collection"
                        v-for="(collection, collectionID) in collections"
                        :key="collectionID"
                    >
                        {{ collection.name }}
                    </li>
                </ul>
            </nav>

            <span
                class="text--disabled"
                v-else
            >
                No collections added yet
            </span>

            <div class="add-collection__button-wrap">
                <button
                    class="add-collection__button"
                    @click="addCollection = !addCollection"
                >
                    <svg
                        width="14"
                        height="14"
                        viewBox="0 0 14 14"
                        fill="none"
                        xmlns="http://www.w3.org/2000/svg"
                    >
                        <path
                            d="M14 7.998H8V13.998H6V7.998H0V5.998H6V-0.00199986H8V5.998H14V7.998Z"
                            fill="#232323"
                        />
                    </svg>

                    Add new Collection
                </button>
            </div>
        </div>
    </div>

    <div class="sections">
        <div
            class="section"
            v-show="addCollection"
        >
            <header class="add-collection__header">
                <h2>New collection: {{ addCollectionLabel }}</h2>

                <button @click="addCollection = !addCollection">
                    <svg
                        width="14"
                        height="14"
                        viewBox="0 0 14 14"
                        fill="none"
                        xmlns="http://www.w3.org/2000/svg"
                        v-if="addCollection"
                    >
                        <path
                            d="M1.4 14L0 12.6L5.6 7L0 1.4L1.4 0L7 5.6L12.6 0L14 1.4L8.4 7L14 12.6L12.6 14L7 8.4L1.4 14Z"
                            fill="black"
                        />
                    </svg>
                </button>
            </header>

            <div class="add-collection__fields">
                <div class="add-collection__field">
                    <label for="addCollectionName">Collection label</label>
                    <input
                        type="text"
                        name="addCollectionLabel"
                        id="addCollectionLabel"
                        v-model="addCollectionLabel"
                        placeholder="eg. Global content"
                        required
                    >
                </div>

                <div class="add-collection__field">
                    <label for="addCollectionName">Collection name (camelcased)</label>
                    <input
                        type="text"
                        name="addCollectionName"
                        id="addCollectionName"
                        v-model="camelizeAddCollectionLabel"
                        :placeholder="customCamelize(addCollectionLabel)"
                        required
                        disabled
                    >
                </div>

                <div
                    class="add-collection__field"
                    v-show="enableCollectionFolder"
                >
                    <label for="addCollectionFolder">Collection folder path</label>
                    <input
                        type="text"
                        name="addCollectionFolder"
                        id="addCollectionFolder"
                        v-model="slugifyAddCollectionFolder"
                        :placeholder="`content/${customSlugify(addCollectionLabel)}`"
                        disabled
                    >
                </div>

                <div
                    class="add-collection__field"
                    v-show="enableCollectionSlug"
                >
                    <label for="addCollectionSlug">Collection slug</label>
                    <input
                        type="text"
                        name="addCollectionSlug"
                        id="addCollectionSlug"
                        v-model="addCollectionSlug"
                        placeholder="{{slug}}"
                        disabled
                    >
                </div>

                <div
                    class="add-collection__field"
                    v-show="enableCollectionCreate"
                >
                    <label for="addCollectionSlug">Collection create</label>
                    <input
                        type="text"
                        name="addCollectionCreate"
                        id="addCollectionCreate"
                        v-model="addCollectionCreate"
                        placeholder="true"
                        disabled
                    >
                </div>
            </div>

            <h3>If you want detail pages (for example blogs) enable all three:</h3>

            <div class="add-collection__checkbox-field">
                <input
                    type="checkbox"
                    name="enableCollectionFolder"
                    id="enableCollectionFolder"
                    v-model="enableCollectionFolder"
                >
                <label for="enableCollectionFolder">Enable folder</label>
            </div>

            <div class="add-collection__checkbox-field">
                <input
                    type="checkbox"
                    name="enableCollectionSlug"
                    id="enableCollectionSlug"
                    v-model="enableCollectionSlug"
                >
                <label for="enableCollectionSlug">Enable slug</label>
            </div>

            <div class="add-collection__checkbox-field">
                <input
                    type="checkbox"
                    name="enableCollectionCreate"
                    id="enableCollectionCreate"
                    v-model="enableCollectionCreate"
                >
                <label for="enableCollectionCreate">Enable option to create subitems</label>
            </div>

            <button @click="addCollectionFile = !addCollectionFile">
                <svg
                    width="14"
                    height="14"
                    viewBox="0 0 14 14"
                    fill="none"
                    xmlns="http://www.w3.org/2000/svg"
                >
                    <path
                        d="M14 7.998H8V13.998H6V7.998H0V5.998H6V-0.00199986H8V5.998H14V7.998Z"
                        fill="#232323"
                    />
                </svg>

                Add new Collection File
            </button>

            <div v-if="addCollectionSection">

            </div>
        </div>

        <ul v-show="collections">
            <li
                v-for="(collection, collectionID) in collections"
                :key="collectionID"
                class="section"
            >
                {{ collection.label }}
            </li>
        </ul>
    </div>
</template>

<script setup>
import { ref, onMounted, computed } from "vue";
import yaml from "js-yaml";

const collections = ref('');
const addCollectionLabel = ref('');
const addCollection = ref(false);
const addCollectionSection = ref(false);
const enableCollectionFolder = ref(false);
const enableCollectionSlug = ref(false);
const enableCollectionCreate = ref(false);
const addCollectionSlug = ref('{{slug}}');

const config = ref({
    backend: { name: "git-gateway", branch: "main" },
    media_folder: "public/upload",
    public_folder: "/upload",
    collections: [],
});

// Create a computed property
const camelizeAddCollectionLabel = computed({
    get() {
        return customCamelize(addCollectionLabel.value);
    },
    set(value) {
        addCollectionLabel.value = value; // Adjust this setter as needed
    }
});

const slugifyAddCollectionFolder = computed({
    get() {
        return 'content/' + customSlugify(addCollectionLabel.value);
    },
    set(value) {
        addCollectionFolder.value = value; // Adjust this setter as needed
    }
});

/* ADD COLLECTION - START */
function saveCollectionToConfig() {
    config.value.collections.push({
        name: "",
        label: "",
        fields: [],
        folder: "", // Initialize with empty string for user input
        create: false, // Initialize as unchecked
        slug: "", // Initialize with empty string for user input
        files: [], // Initialize files array to enable file addition
        collapsed: true,
    });
}

function addField(collection) {
    if (!collection.fields) {
        collection.fields = [];
    }
    collection.fields.push({
        name: "",
        label: "",
        widget: "string",
        required: false,
    }); // Explicitly set required to false
}
/* ADD COLLECTION - END */

function customCamelize(str) {
    return str
        .toLowerCase()
        .replace(/[^a-zA-Z0-9]+(.)/g, (m, chr) => chr.toUpperCase());
}

function customSlugify(str) {
    return str
        .toLowerCase()
        .replace(/[^a-zA-Z0-9]+/g, '-'); // Replace non-alphanumeric characters with dashes
}
</script>