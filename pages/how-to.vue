<template>
  <div>
    <div class="container">
      <div class="page-inner">
        <div class="sidebar">
          <h2>Structure</h2>
          <section class="section">
            <ul>
              <li><strong>Collections</strong> are like categories</li>
              <li><strong>Files</strong> are like subcategories</li>
              <li>
                You can have multiple Files per Collection. Those are
                pre-defined static datasets.
                <span class="label">[screenshot 1,2]</span>
              </li>
              <li>
                Or you can have the <strong>create</strong> option, so that you
                can create detail pages, for example blog articles. Those are
                dynamic datasets.
                <span class="label">[screenshot 3]</span>
              </li>
              <li>
                For List and Object fields you can make
                <strong>subfields</strong>
              </li>
            </ul>
          </section>

          <h3>More info</h3>

          <section class="section">
            <a href="https://decapcms.org/docs/intro/" target="_blank">
              Decap CMS Docs
              <svg
                width="14"
                height="10"
                viewBox="0 0 18 14"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  fill-rule="evenodd"
                  clip-rule="evenodd"
                  d="M10.47 0.47C10.6106 0.329549 10.8012 0.25066 11 0.25066C11.1988 0.25066 11.3894 0.329549 11.53 0.47L17.53 6.47C17.6705 6.61063 17.7493 6.80125 17.7493 7C17.7493 7.19875 17.6705 7.38937 17.53 7.53L11.53 13.53C11.4613 13.6037 11.3785 13.6628 11.2865 13.7038C11.1945 13.7448 11.0952 13.7668 10.9945 13.7686C10.8938 13.7704 10.7938 13.7518 10.7004 13.7141C10.607 13.6764 10.5222 13.6203 10.451 13.549C10.3797 13.4778 10.3236 13.393 10.2859 13.2996C10.2482 13.2062 10.2296 13.1062 10.2314 13.0055C10.2332 12.9048 10.2552 12.8055 10.2962 12.7135C10.3372 12.6215 10.3963 12.5387 10.47 12.47L15.19 7.75H1C0.801088 7.75 0.610322 7.67098 0.46967 7.53033C0.329018 7.38968 0.25 7.19891 0.25 7C0.25 6.80109 0.329018 6.61032 0.46967 6.46967C0.610322 6.32902 0.801088 6.25 1 6.25H15.19L10.47 1.53C10.3295 1.38937 10.2507 1.19875 10.2507 1C10.2507 0.801249 10.3295 0.610625 10.47 0.47Z"
                  fill="black"
                />
              </svg>
            </a>
          </section>
        </div>

        <div class="sections">
          <h2>Config example:</h2>
          <section class="section">
            <div class="docs">
              <div class="docs__section">
                <ul>
                  <li>
                    General content (Collection)
                    <span class="label">[screenshot 1]</span>
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
                    <img src="~/assets/img/general-content.png" />
                  </li>

                  <li>
                    <p class="label">Screenshot 2: Pages</p>
                    <img src="~/assets/img/pages.png" />
                  </li>

                  <li>
                    <p class="label">
                      Screenshot 3: Detail pages (for example Blogs or
                      Portfolio)
                    </p>
                    <img src="~/assets/img/detail-pages.png" />
                  </li>
                </ul>
              </div>
            </div>
          </section>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from "vue";
import yaml from "js-yaml";

const Collections = ref("");
const addCollectionLabel = ref("");
const addCollection = ref(false);
const addCollectionSection = ref(false);
const enableCollectionFolder = ref(false);
const enableCollectionSlug = ref(false);
const enableCollectionCreate = ref(false);
const addCollectionSlug = ref("{{slug}}");

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
  },
});

const slugifyAddCollectionFolder = computed({
  get() {
    return "content/" + customSlugify(addCollectionLabel.value);
  },
  set(value) {
    addCollectionFolder.value = value; // Adjust this setter as needed
  },
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
  return str.toLowerCase().replace(/[^a-zA-Z0-9]+/g, "-"); // Replace non-alphanumeric characters with dashes
}
</script>
