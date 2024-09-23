<template>
  <div class="container">
    <div class="page-inner">
      <!-- Sidebar with default configuration -->
      <div class="sidebar">
        <h2>Default configuration</h2>

        <div class="sections">
          <!-- Backend Configuration -->
          <section class="section">
            <div class="section__inner">
              <h3>Backend Configuration</h3>

              <div class="input__fields">
                <div class="input__field">
                  <label>Backend Name</label>
                  <input
                    v-model="config.backend.name"
                    placeholder="Backend Name"
                  />
                </div>

                <div class="input__field">
                  <label>Branch name</label>
                  <input
                    v-model="config.backend.branch"
                    placeholder="Branch name"
                  />
                </div>
              </div>
            </div>
          </section>

          <!-- Media Configuration -->
          <section class="section">
            <div class="section__inner">
              <h3>Media Configuration</h3>
              <div class="input__fields">
                <div class="input__field">
                  <label>Media Folder</label>
                  <input
                    v-model="config.media_folder"
                    placeholder="Media Folder"
                  />
                </div>
                <div class="input__field">
                  <label>Public Upload Folder</label>
                  <input
                    v-model="config.public_folder"
                    placeholder="Public Upload Folder"
                  />
                </div>
              </div>
            </div>
          </section>

          <!-- SEO Configuration -->
          <section class="section">
            <div class="section__inner">
              <h3>SEO Configuration</h3>
              <textarea
                v-model="seoConfigYaml"
                rows="10"
                cols="50"
                class="sidebar__textarea"
              ></textarea>
            </div>
          </section>

          <section class="section">
            <h3>Save config.yml</h3>
            <button @click="generateConfig" class="button">
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
              Save config
            </button>

            <label>Save config.yml in /public/admin/ folder</label>
          </section>
        </div>
      </div>

      <!-- Collections -->
      <div class="sections">
        <div class="sections__header">
          <h2>Collections</h2>

          <button @click="addCollection" class="button">
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
            Add new collection
          </button>
        </div>

        <div
          v-for="(collection, collectionIndex) in config.collections"
          :key="collectionIndex"
          class="section collection"
        >
          <div class="collection__header" @click="toggleCollapse(collection)">
            <h3>
              Collection #{{ collectionIndex + 1 }}:
              {{ collection.label || "Unnamed" }}
            </h3>

            <button>
              <span v-if="collection.collapsed">+</span>
              <span v-else>-</span>
            </button>
          </div>

          <div v-show="!collection.collapsed" class="collection__details">
            <div class="input__fields">
              <div class="input__field">
                <label>Collection Label:</label>
                <input v-model="collection.label" placeholder="Enter label" />
              </div>

              <div class="input__field">
                <label>Collection Name:</label>
                <input
                  v-model="collection.name"
                  :placeholder="customCamelize(collection.label)"
                />
              </div>

              <div class="input__field">
                <label>Folder:</label>
                <input
                  v-model="collection.folder"
                  :placeholder="`content/${customSlugify(collection.label)}`"
                />
              </div>

              <div class="input__field" v-if="collection.create == true">
                <label>Slug:</label>
                <input v-model="collection.slug" placeholder="{{slug}}" />
              </div>
            </div>

            <div class="input__field">
              <label class="input-label checkbox-label">
                <input
                  v-model="collection.create"
                  type="checkbox"
                  class="checkbox"
                />
                Enable detail pages
              </label>
            </div>

            <button
              @click="addFile(collection)"
              class="button"
              v-if="collection.create != true"
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
              Add File
            </button>

            <div
              v-if="collection.files && collection.create != true"
              class="files"
            >
              <h4>Collection files:</h4>

              <ul class="files__inner">
                <li
                  v-for="(file, fileIndex) in collection.files"
                  :key="fileIndex"
                  class="file"
                >
                  <h5>File #{{ fileIndex + 1 }}: {{ file.label }}</h5>

                  <div class="input__fields file__input-fields">
                    <div class="input__field">
                      <label>File label</label>
                      <input v-model="file.label" placeholder="File Label" />
                    </div>

                    <div class="input__field">
                      <label>File name</label>
                      <input
                        v-model="file.name"
                        :placeholder="customCamelize(file.label)"
                      />
                    </div>

                    <div class="input__field">
                      <label>File path</label>
                      <input v-model="file.file" placeholder="File Path" />
                    </div>
                  </div>

                  <!-- Display fields for each file -->
                  <div v-if="file.fields" class="file__fields">
                    <h6>File fields:</h6>

                    <div class="fields__header">
                      <label>Field label</label>
                      <label>Field name</label>
                      <label>Field label</label>
                      <label>Required</label>
                    </div>

                    <ul class="file__fields__inner">
                      <li
                        v-for="(field, fieldIndex) in file.fields"
                        :key="fieldIndex"
                        class="file__field"
                      >
                        <template v-if="!isAnchorReference(field)">
                          <div class="input__fields">
                            <div class="input__field">
                              <input
                                v-model="field.label"
                                placeholder="Field Label"
                              />
                            </div>

                            <div class="input__field">
                              <input
                                v-model="field.name"
                                :placeholder="customCamelize(field.label)"
                              />
                            </div>

                            <div class="input__field">
                              <select v-model="field.widget">
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
                                v-model="field.required"
                                type="checkbox"
                                id="required"
                              />
                              <label for="required">Required</label>
                            </div>
                          </div>
                        </template>

                        <template v-else-if="field === '*seo'">
                          <div class="seo-fields">
                            <h5 class="font-bold">SEO Fields:</h5>
                            <div
                              v-for="seoField in seoFields"
                              :key="seoField.name"
                            >
                              <input
                                v-model="seoField.value"
                                :placeholder="seoField.label"
                              />
                            </div>
                          </div>
                        </template>

                        <template v-else>
                          <p>
                            <strong> SEO Fields (referenced) </strong>
                          </p>
                        </template>

                        <!-- Display subfields for each file field -->
                        <div v-if="field.fields" class="subfields__wrapper">
                          <h6 class="subfields__label">
                            {{ field.label }} Subfields:
                          </h6>

                          <ul class="subfields">
                            <li
                              v-for="(subfield, subfieldIndex) in field.fields"
                              :key="subfieldIndex"
                              class="input__fields"
                            >
                              <div class="input__field">
                                <input
                                  v-model="subfield.label"
                                  placeholder="Subfield Label"
                                />
                              </div>

                              <div class="input__field">
                                <input
                                  v-model="subfield.name"
                                  :placeholder="customSlugify(subfield.label)"
                                />
                              </div>

                              <div class="input__field">
                                <select v-model="subfield.widget">
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
                            </li>
                          </ul>

                          <button
                            @click="addSubfield(field)"
                            class="button"
                            v-if="
                              field.widget == 'list' || field.widget == 'object'
                            "
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
                            Add Subfield
                          </button>
                        </div>
                      </li>
                    </ul>

                    <button
                      @click="addField(file)"
                      class="file__fields__button"
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
                      Add Field
                    </button>
                  </div>
                </li>
              </ul>

              <button @click="addFile(collection)" class="button">
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
                Add File
              </button>
            </div>

            <!-- Collection fields -->
            <div v-if="collection.fields">
              <h3 class="collection__fields__header">Collection fields:</h3>

              <div class="fields__header collection__labels">
                <label>Field label</label>
                <label>Field name</label>
                <label>Field label</label>
                <label>Required</label>
              </div>

              <ul class="collection__fields">
                <li
                  v-for="(field, fieldIndex) in collection.fields"
                  :key="fieldIndex"
                  draggable="true"
                  @dragstart="dragStart($event, fieldIndex, collectionIndex)"
                  @drop="drop($event, fieldIndex, collectionIndex)"
                  @dragover.prevent
                  class="draggable-field"
                >
                  <template v-if="!isAnchorReference(field)">
                    <div class="draggable-field__inner">
                      <div class="draggable-field__icon">
                        <svg
                          width="6"
                          height="10"
                          viewBox="0 0 6 10"
                          fill="none"
                          xmlns="http://www.w3.org/2000/svg"
                        >
                          <path
                            d="M6 4H0L3 0L6 4ZM0 6H6L3 10L0 6Z"
                            fill="black"
                          />
                        </svg>
                      </div>

                      <div class="input__fields">
                        <div class="input__field">
                          <input
                            v-model="field.label"
                            placeholder="Field Label"
                          />
                        </div>

                        <div class="input__field">
                          <input
                            v-model="field.name"
                            :placeholder="customCamelize(field.label)"
                          />
                        </div>

                        <div class="input__field">
                          <select v-model="field.widget">
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

                        <div class="input__field">
                          <div class="checkbox-container">
                            <input
                              type="checkbox"
                              v-model="field.required"
                              :id="'required-' + fieldIndex"
                            />
                            <label :for="'required-' + fieldIndex"
                              >Required</label
                            >
                          </div>
                        </div>
                      </div>
                    </div>

                    <!-- Display subfields if they exist -->
                    <div v-if="field.fields" class="subfields__wrapper">
                      <strong>{{ field.label }} subfields</strong>

                      <div class="fields__header subfields__header">
                        <label>Field label</label>
                        <label>Field name</label>
                        <label>Field label</label>
                        <label>Required</label>
                      </div>

                      <ul class="subfields">
                        <li
                          v-for="(subfield, subfieldIndex) in field.fields"
                          :key="subfieldIndex"
                          class="input__fields draggable-field"
                          draggable="true"
                          @dragstart="dragStart($event, subfieldIndex)"
                          @drop="drop($event, subfieldIndex)"
                          @dragover.prevent
                        >
                          <div class="draggable-field__inner">
                            <div class="draggable-field__icon">
                              <svg
                                width="6"
                                height="10"
                                viewBox="0 0 6 10"
                                fill="none"
                                xmlns="http://www.w3.org/2000/svg"
                              >
                                <path
                                  d="M6 4H0L3 0L6 4ZM0 6H6L3 10L0 6Z"
                                  fill="black"
                                />
                              </svg>
                            </div>

                            <div class="input__field">
                              <input
                                v-model="subfield.label"
                                placeholder="Subfield Label"
                              />
                            </div>

                            <div class="input__field">
                              <input
                                v-model="subfield.name"
                                :placeholder="customCamelize(subfield.label)"
                              />
                            </div>

                            <div class="input__field">
                              <select v-model="subfield.widget">
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

                            <div class="input__field">
                              <div class="checkbox-container">
                                <input
                                  type="checkbox"
                                  v-model="subfield.required"
                                  :id="'required-' + subfieldIndex"
                                />
                                <label :for="'required-' + subfieldIndex"
                                  >Required</label
                                >
                              </div>
                            </div>
                          </div>
                        </li>
                      </ul>
                    </div>

                    <button
                      v-if="field.widget == 'list' || field.widget == 'object'"
                      @click="addSubfield(field)"
                      class="collection__fields__button"
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
                      Add Subfield
                    </button>
                  </template>

                  <template v-else>
                    <p>
                      <strong> SEO Fields (referenced) </strong>
                    </p>
                  </template>
                </li>
              </ul>

              <button @click="addField(collection)" class="button">
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
                Add Field
              </button>

              <button @click="toggleSEO(collection)" class="button">
                <svg
                  width="14"
                  height="14"
                  viewBox="0 0 14 14"
                  fill="none"
                  xmlns="http://www.w3.org/2000/svg"
                  v-if="!hasSEO(collection)"
                >
                  <path
                    d="M14 7.998H8V13.998H6V7.998H0V5.998H6V-0.00199986H8V5.998H14V7.998Z"
                    fill="#232323"
                  />
                </svg>

                <svg
                  v-else-if="hasSEO(collection)"
                  width="14"
                  height="2"
                  viewBox="0 0 14 2"
                  fill="none"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    d="M14 1.99799H0V-0.00201416H14V1.99799Z"
                    fill="black"
                  />
                </svg>

                {{ hasSEO(collection) ? "Remove SEO" : "Add SEO" }}
              </button>
            </div>

            <button
              @click="addField(collection)"
              class="button"
              v-if="collection.create == true && !collection.fields"
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
              Add Field
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import yaml from "js-yaml";

// Set the page title
useHead({
  title: "Decap CMS config.yml configurator",
});

/* DEFAULT CONFIG - START */
const config = ref({
  backend: { name: "git-gateway", branch: "main" },
  media_folder: "public/upload",
  public_folder: "/upload",
  collections: [],
});
const seoConfigYaml = ref("");
/* DEFAULT CONFIG - END */

// CAMELIZE STRINGS (thisIsCamelized) - START
function customCamelize(str) {
  if (!str) return "";
  return str
    .toLowerCase()
    .replace(/[^a-zA-Z0-9]+(.)/g, (m, chr) => chr.toUpperCase());
}
// CAMELIZE STRINGS (thisIsCamelized) - END

// SLUGIFY STRINGS (this-is-a-slug) - START
function customSlugify(str) {
  if (!str) return ""; // Return an empty string if str is undefined or null
  return str.toLowerCase().replace(/[^a-zA-Z0-9]+/g, "-"); // Replace non-alphanumeric characters with dashes
}
// SLUGIFY STRINGS (this-is-a-slug) - END

/* SEO FIELDS - START */
const seoFields = ref([
  {
    name: "metaTitle",
    label: "SEO Title",
    widget: "string",
    value: "",
    required: true,
  },
  {
    name: "metaDescription",
    label: "SEO Description",
    widget: "string",
    value: "",
    required: true,
  },
  { name: "metaImage", label: "SEO image", widget: "image", value: "" },
  { name: "keywords", label: "keywords", widget: "string", value: "" },
]);
/* SEO FIELDS - END */

/* DRAG AND DROP FIELDS - START */
// Define the reactive references for indices
let draggedIndex = ref(-1);
let draggedCollectionIndex = ref(-1);
let draggedFileIndex = ref(-1);

function dragStart(event, index, collectionIndex) {
  console.log("Dragging from:", { index, collectionIndex });
  draggedIndex.value = index;
  draggedCollectionIndex.value = collectionIndex;
  event.dataTransfer.effectAllowed = "move";
  event.dataTransfer.setData("text/plain", null); // Necessary for Firefox
}

function dragOver(event) {
  event.preventDefault(); // Necessary to allow dropping
}

function drop(event, targetIndex, collectionIndex) {
  event.preventDefault();
  console.log("Dropping into:", { targetIndex, collectionIndex });

  if (typeof collectionIndex === "undefined") {
    console.error("Collection index is undefined during drop.");
    return;
  }

  if (
    collectionIndex >= 0 &&
    collectionIndex < config.value.collections.length
  ) {
    const collection = config.value.collections[collectionIndex];
    if (collection && collection.fields) {
      const originalIndex = draggedIndex.value;
      const originalCollectionIndex = draggedCollectionIndex.value;

      if (originalCollectionIndex === collectionIndex) {
        let targetList = collection.fields;
        if (targetIndex >= 0 && targetIndex < targetList.length) {
          if (originalIndex !== targetIndex) {
            const itemToMove = targetList.splice(originalIndex, 1)[0];
            targetList.splice(targetIndex, 0, itemToMove);
          }
        } else {
          console.error("Invalid target index:", targetIndex);
        }
      } else {
        console.error("Attempt to drop into a different collection");
      }
    } else {
      console.error("Collection has no fields or is undefined:", collection);
    }
  } else {
    console.error("Invalid collection index:", collectionIndex);
  }
}
/* DRAG AND DROP FIELDS - END */

/* READ EXISTING CONFIG - START */
onMounted(async () => {
  let parsedYaml;
  try {
    const response = await fetch("/admin/config.yml");
    const yamlText = await response.text();
    parsedYaml = yaml.load(yamlText);
    // Ensure all collections are collapsed by default
    if (parsedYaml.collections) {
      parsedYaml.collections = parsedYaml.collections.map((collection) => ({
        ...collection,
        collapsed: true,
      }));
    }
    config.value = parsedYaml;
    seoConfigYaml.value = yaml.dump(parsedYaml.seo);
  } catch (error) {
    console.error("Error loading config:", error);
  }
  if (parsedYaml && parsedYaml.seo) {
    const parsedSeoFields = yaml.load(seoConfigYaml.value);
    seoFields.value.forEach((field) => {
      field.value = parsedSeoFields[field.name] || "";
    });
  }
});
/* READ EXISTING CONFIG - END */

/* ADD COLLECTION - START */
function addCollection() {
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

/* ADD SUBFIELD - START */
function addSubfield(field) {
  if (!field.fields) {
    field.fields = [];
  }
  field.fields.push({ name: "", label: "", widget: "string", required: false }); // Explicitly set required to false
}
/* ADD SUBFIELD - END */

/* ADD FILE - START */
function addFile(collection) {
  if (!collection.files) collection.files = [];
  collection.files.push({ name: "", label: "", file: "", fields: [] });
}
/* ADD FILE - END */

/* IS ANCHOR REFERENCE - START */
function isAnchorReference(field) {
  return typeof field === "string" && field.startsWith("*");
}

function hasSEO(collection) {
  return (
    collection.fields &&
    collection.fields.some((field) => isAnchorReference(field))
  );
}

function toggleSEO(collection) {
  if (!collection.fields) {
    collection.fields = [];
  }
  const seoIndex = collection.fields.findIndex(isAnchorReference);
  if (seoIndex !== -1) {
    collection.fields.splice(seoIndex, 1);
  } else {
    collection.fields.push("*seo");
  }
}
/* IS ANCHOR REFERENCE - END */

/* GENERATE CONFIG - START */
function generateConfig() {
  const finalConfig = {
    ...config.value,
    collections: config.value.collections.map(({ collapsed, ...rest }) => rest), // Exclude 'collapsed' from the output
    seo: yaml.load(seoConfigYaml.value),
  };

  const yamlString = yaml.dump(finalConfig);
  const blob = new Blob([yamlString], { type: "text/yaml" });
  const url = URL.createObjectURL(blob);

  const link = document.createElement("a");
  link.href = url;
  link.download = "config.yml";
  link.click();

  URL.revokeObjectURL(url);
}
/* GENERATE CONFIG - END */

/* COLLAPSIBLE SECTION - START */
function toggleCollapse(collection) {
  collection.collapsed = !collection.collapsed;
}
/* COLLAPSIBLE SECTION - END */
</script>
