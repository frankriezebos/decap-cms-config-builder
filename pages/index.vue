<template>
    <div>
        <div class="container">
            <div class="page-inner">
                <!-- Sidebar -->
                <div class="sidebar">
                    <div class="sidebar__section">
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

                            <div class="input__button-wrap">
                                <button
                                    class="input__button"
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

                    <div class="sidebar__section">
                        <header class="section__header">
                            <h2>General settings</h2>
                        </header>

                        <div class="section__inner">
                            <div class="input__fields">
                                <div class="input__field">
                                    <label for="configBackendName">Backend name</label>
                                    <input
                                        type="text"
                                        name="configBackendName"
                                        id="configBackendName"
                                        v-model="config.backend.name"
                                    >
                                </div>

                                <div class="input__field">
                                    <label for="configBackendBranch">Backend branch</label>
                                    <input
                                        type="text"
                                        name="configBackendBranch"
                                        id="configBackendBranch"
                                        v-model="config.backend.branch"
                                    >
                                </div>

                                <div class="input__field">
                                    <label for="configMediaFolder">Media folder</label>
                                    <input
                                        type="text"
                                        name="configMediaFolder"
                                        id="configMediaFolder"
                                        v-model="config.media_folder"
                                    >
                                </div>

                                <div class="input__field">
                                    <label for="configPublicFolder">Public folder</label>
                                    <input
                                        type="text"
                                        name="configPublicFolder"
                                        id="configPublicFolder"
                                        v-model="config.public_folder"
                                    >
                                </div>
                            </div>
                        </div>
                    </div>

                    <div class="sidebar__section">
                        <button>
                            <svg
                                width="16"
                                height="16"
                                viewBox="0 0 16 16"
                                fill="none"
                                xmlns="http://www.w3.org/2000/svg"
                            >
                                <path
                                    d="M16 3.423V16H0V0H12.577L16 3.423ZM7.996 12.538C8.55 12.538 9.02233 12.344 9.413 11.956C9.80367 11.568 9.99933 11.097 10 10.543C10.0007 9.989 9.80667 9.51633 9.418 9.125C9.02933 8.73367 8.55833 8.53833 8.005 8.539C7.45167 8.53967 6.97867 8.73333 6.586 9.12C6.19333 9.50667 5.998 9.97833 6 10.535C6.002 11.0917 6.196 11.5643 6.582 11.953C6.968 12.3417 7.43933 12.5373 7.996 12.54M2.769 5.77H10.192V2.77H2.77L2.769 5.77Z"
                                    fill="black"
                                />
                            </svg>
                            Save config.yml
                        </button>

                        <p>Save within /public/admin/ folder</p>
                    </div>
                </div>

                <!-- Collections -->
                <div class="sections">
                    <div
                        class="section"
                        v-show="addCollection"
                    >
                        <header class="input__header">
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

                        <div class="input__fields">
                            <div class="input__field">
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

                            <div class="input__field">
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
                                class="input__field"
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
                                class="input__field"
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
                                class="input__field"
                                v-show="enableCollectionCreate"
                            >
                                <label for="addCollectionSlug">Option to create subitems</label>
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

                        <div class="input__checkbox-field">
                            <input
                                type="checkbox"
                                name="enableCollectionFolder"
                                id="enableCollectionFolder"
                                v-model="enableCollectionFolder"
                            >
                            <label for="enableCollectionFolder">Enable folder (all md files will be saved within this
                                folder)</label>
                        </div>

                        <div class="input__checkbox-field">
                            <input
                                type="checkbox"
                                name="enableCollectionSlug"
                                id="enableCollectionSlug"
                                v-model="enableCollectionSlug"
                            >
                            <label for="enableCollectionSlug">Enable slug (neccesary for detail pages)</label>
                        </div>

                        <div class="input__checkbox-field">
                            <input
                                type="checkbox"
                                name="enableCollectionCreate"
                                id="enableCollectionCreate"
                                v-model="enableCollectionCreate"
                            >
                            <label for="enableCollectionCreate">Enable Create (option to create subitems)</label>
                        </div>

                        <button @click="addNewFile">
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

                        <!-- Add Collection File -->
                        <div
                            class="file"
                            v-for="(file, fileIndex) in files"
                            :key="fileIndex"
                        >
                            <div class="input__fields">
                                <div class="input__field">
                                    <label :for="'addFileLabel' + fileIndex">File label</label>

                                    <input
                                        type="text"
                                        v-model="file.label"
                                        placeholder="eg. Buttons"
                                        required
                                        :name="'addFileLabel' + fileIndex"
                                        :id="'addFileLabel' + fileIndex"
                                    >
                                </div>

                                <div class="input__field">
                                    <label :for="'addFilePath' + fileIndex">File path</label>

                                    <input
                                        type="text"
                                        :name="'addFilePath' + fileIndex"
                                        :id="'addFilePath' + fileIndex"
                                        v-model="file.path"
                                        :placeholder="`content${customSlugify(addCollectionLabel)}/${customSlugify(addFileLabel)}.md`"
                                        disabled
                                    >
                                </div>

                                <div class="input__field">
                                    <label for="addFileName">File name (camelcased)</label>

                                    <input
                                        type="text"
                                        :name="'addFileName' + fileIndex"
                                        :id="'addFileName' + fileIndex"
                                        v-model="file.name"
                                        :placeholder="customCamelize(addCollectionLabel) + addFileLabel"
                                        disabled
                                    >
                                </div>

                                <div>
                                    <button @click="removeFile(fileIndex)">
                                        <svg
                                            width="14"
                                            height="14"
                                            viewBox="0 0 14 14"
                                            fill="none"
                                            xmlns="http://www.w3.org/2000/svg"
                                        >
                                            <path
                                                d="M1.4 14L0 12.6L5.6 7L0 1.4L1.4 0L7 5.6L12.6 0L14 1.4L8.4 7L14 12.6L12.6 14L7 8.4L1.4 14Z"
                                                fill="black"
                                            />
                                        </svg>
                                    </button>
                                </div>
                            </div>

                            <div class="file__fields">
                                <h3>Add Fields to this File</h3>

                                <!-- Display fields for each file dynamically -->
                                <div
                                    class="input__fields file__fields"
                                    v-for="(field, fieldIndex) in fileFields"
                                    :key="fieldIndex"
                                >
                                    <div class="input__field">
                                        <label :for="'fileFieldLabel' + fieldIndex">File field label</label>
                                        <input
                                            type="text"
                                            :name="'fileFieldLabel' + fieldIndex"
                                            :id="'fileFieldLabel' + fieldIndex"
                                            v-model="field.label"
                                            placeholder="Field Label"
                                        />
                                    </div>

                                    <div class="input__field">
                                        <label :for="'fileFieldName' + fieldIndex">File field name</label>
                                        <input
                                            type="text"
                                            :name="'fileFieldName' + fieldIndex"
                                            :id="'fileFieldName' + fieldIndex"
                                            v-model="field.name"
                                            :placeholder="customCamelize(field.label)"
                                            disabled
                                        />
                                    </div>

                                    <div class="input__field">
                                        <label :for="'fileFieldType' + fieldIndex">File field type</label>
                                        <select
                                            :id="'fileFieldType' + fieldIndex"
                                            v-model="field.type"
                                            :name="'fileFieldType' + fieldIndex"
                                            @change="handleFieldTypeChange(field, $event.target.value)"
                                        >
                                            <option value="string">String</option>
                                            <option value="text">Text</option>
                                            <option value="datetime">DateTime</option>
                                            <option value="markdown">Markdown</option>
                                            <option value="boolean">Boolean</option>
                                            <option value="code">Code</option>
                                            <option value="color">Color</option>
                                            <option value="file">File</option>
                                            <option value="hidden">Hidden</option>
                                            <option value="image">Image</option>
                                            <option value="list">List</option>
                                            <option value="map">Map</option>
                                            <option value="number">Number</option>
                                            <option value="object">Object</option>
                                            <option value="relation">Relation</option>
                                            <option value="select">Select</option>
                                        </select>
                                    </div>

                                    <div class="input__checkbox-field">
                                        <input
                                            type="checkbox"
                                            :id="'required' + fieldIndex"
                                            v-model="field.required"
                                        />
                                        <label :for="'required' + fieldIndex">Required</label>
                                    </div>

                                    <div>
                                        <button @click="removeField(fileIndex, fieldIndex)">
                                            <svg
                                                width="14"
                                                height="14"
                                                viewBox="0 0 14 14"
                                                fill="none"
                                                xmlns="http://www.w3.org/2000/svg"
                                            >
                                                <path
                                                    d="M1.4 14L0 12.6L5.6 7L0 1.4L1.4 0L7 5.6L12.6 0L14 1.4L8.4 7L14 12.6L12.6 14L7 8.4L1.4 14Z"
                                                    fill="black"
                                                />
                                            </svg>
                                        </button>
                                    </div>
                                </div>

                                <button @click="addNewField">
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

                                    Add new field
                                </button>
                            </div>
                        </div>
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
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted, computed } from "vue";
import yaml from "js-yaml";

// Collection
const collections = ref('');
const addCollectionLabel = ref('');
const addCollection = ref(false);
const addCollectionCreate = ref(false);
const enableCollectionFolder = ref(false);
const enableCollectionSlug = ref(false);
const enableCollectionCreate = ref(false);
const addCollectionSlug = ref('{{slug}}');

// Collection > File
const files = ref([]);

// Collection > File > Fields
const fileFields = ref([]);

// Collection > File > Fields > Subfields
const fileSubFields = ref([]);

// Camelize strings (thisIsCamelized)
function customCamelize(str) {
    return str
        .toLowerCase()
        .replace(/[^a-zA-Z0-9]+(.)/g, (m, chr) => chr.toUpperCase());
}

// Slugify strings (this-is-a-slug)
function customSlugify(str) {
    return str
        .toLowerCase()
        .replace(/[^a-zA-Z0-9]+/g, '-'); // Replace non-alphanumeric characters with dashes
}

// General settings
const config = ref({
    backend: { name: "git-gateway", branch: "main" },
    media_folder: "public/upload",
    public_folder: "/upload",
    collections: [],
});

// Camelize collection label
const camelizeAddCollectionLabel = computed({
    get() {
        return customCamelize(addCollectionLabel.value);
    },
    set(value) {
        addCollectionLabel.value = value; // Adjust this setter as needed
    }
});

// Slugify collection folder
const slugifyAddCollectionFolder = computed({
    get() {
        return 'content/' + customSlugify(addCollectionLabel.value);
    },
    set(value) {
        addCollectionFolder.value = value; // Adjust this setter as needed
    }
});

// List and Object fields can have subfields
function handleFieldTypeChange(field, type) {
    field.type = type;
    if (type === 'list' || type === 'object') {
        // Enable subfields if the type is 'list' or 'object'
        Vue.set(field, 'subfields', []);
    } else {
        // Remove subfields if the type is changed from 'list' or 'object' to something else
        if (field.subfields) {
            Vue.delete(field, 'subfields');
        }
    }
}

// Add new file
function addNewFile() {
    files.value.push({
        name: '',
        path: '',
        label: '',
    });
}

// Add new field
function addNewField() {
    fileFields.value.push({
        label: '',
        name: '',
        type: 'string', // default type
        required: false
    });
}

// Function to remove a file
function removeFile(fileIndex) {
    files.value.splice(fileIndex, 1);
}

// Function to remove a field from fileFields
function removeField(fileIndex, fieldIndex) {
    files.value.fileIndex.fileFields.value.splice(fieldIndex, 1);
}

// Save collection to config
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
</script>