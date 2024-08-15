<template>
    <div>
        <div class="container">
            <div class="page-inner">
                <div class="sections">
                    <div class="section">
                        <h2>About Decap CMS</h2>
                        <p></p>
                    </div>
                </div>
            </div>
        </div>
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