<template>
    <div>
        <div class="container">
            <div class="page-inner">
                <div class="sections">
                    <div class="section">
                        <h2>Structure</h2>
                        <ul>
                            <li><strong>Collections</strong> are like categories within Decap CMS.</li>
                            <li><strong>Files</strong> are like subcategories within Decap CMS.</li>
                            <li>You can have multiple Files
                                per Collection.</li>
                            <li>Or you can have the <strong>create</strong> option, so that you can create detail pages,
                                for
                                example blog articles. <span class="label">[screenshot 3]</span></li>
                            <li>For List and Object fields you can make <strong>subfields</strong></li>
                        </ul>

                        <div class="docs">
                            <div class="docs__section">
                                <h2>Config example:</h2>
                                <ul>
                                    <li>
                                        General content (Collection) <span class="label">[screenshot 1]</span>
                                        <ul>
                                            <li>
                                                Buttons (File)
                                                <ul>
                                                    <li>Label (String field)</li>
                                                    <li>Link (String field)</li>
                                                </ul>
                                            </li>
                                            <li>
                                                General labels (File)
                                                <ul>
                                                    <li>Label 1 (String field)</li>
                                                    <li>Label 2 (String field)</li>
                                                    <li>Label 3 (String field)</li>
                                                </ul>
                                            </li>
                                            <li>
                                                Footer (File)
                                                <ul>
                                                    <li>Copyright text (String field)</li>
                                                </ul>
                                            </li>
                                            <li>
                                                Socials (File)
                                                <ul>
                                                    <li>
                                                        Socials (List field)
                                                        <ul>
                                                            <li>Title (String field)</li>
                                                            <li>Link (String field)</li>
                                                            <li>Icon (Image field)</li>
                                                        </ul>
                                                    </li>
                                                </ul>
                                            </li>
                                        </ul>
                                    </li>

                                    <li>
                                        Pages (Collection) <span class="label">[screenshot 2]</span>
                                        <ul>
                                            <li>
                                                Home (File)
                                                <ul>
                                                    <li>Title (String field)</li>
                                                    <li>Description (Text field)</li>
                                                </ul>
                                            </li>
                                            <li>
                                                About (File)
                                                <ul>
                                                    <li>Title (String field)</li>
                                                    <li>Description (Text field)</li>
                                                </ul>
                                            </li>
                                        </ul>
                                    </li>

                                    <li>
                                        Blogs (Collection) <span class="label">[screenshot 3]</span>
                                        <ul>
                                            <li>
                                                Blog 1 (Created item)
                                                <ul>
                                                    <li>Title (String field)</li>
                                                    <li>Excerpt (Text field)</li>
                                                    <li>Image (Image field)</li>
                                                    <li>Content (Markdown field)</li>
                                                </ul>
                                            </li>

                                            <li>
                                                Blog 2 (Created item)
                                                <ul>
                                                    <li>Title (String field)</li>
                                                    <li>Excerpt (Text field)</li>
                                                    <li>Image (Image field)</li>
                                                    <li>Content (Markdown field)</li>
                                                </ul>
                                            </li>

                                            <li>
                                                Blog 3 (Created item)
                                                <ul>
                                                    <li>Title (String field)</li>
                                                    <li>Excerpt (Text field)</li>
                                                    <li>Image (Image field)</li>
                                                    <li>Content (Markdown field)</li>
                                                </ul>
                                            </li>
                                        </ul>
                                    </li>
                                </ul>
                            </div>

                            <div class="docs__section">
                                <ul class="docs__images">
                                    <li>
                                        <p class="label">Screenshot 1: General Content</p>
                                        <img src="~/assets/img/general-content.png">
                                    </li>

                                    <li>
                                        <p class="label">Screenshot 2: Pages</p>
                                        <img src="~/assets/img/pages.png">
                                    </li>

                                    <li>
                                        <p class="label">Screenshot 3: Detail pages (for example Blogs or Portfolio)</p>
                                        <img src="~/assets/img/detail-pages.png">
                                    </li>
                                </ul>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted, computed } from "vue";
import yaml from "js-yaml";

const Collections = ref('');
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
    Collections: [],
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
    config.value.Collections.push({
        name: "",
        label: "",
        fields: [],
        folder: "", // Initialize with empty string for user input
        create: false, // Initialize as unchecked
        slug: "", // Initialize with empty string for user input
        Files: [], // Initialize Files array to enable File addition
        collapsed: true,
    });
}

function addField(Collection) {
    if (!Collection.fields) {
        Collection.fields = [];
    }
    Collection.fields.push({
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