<template>
    <div>
        <div class="container">
            <div class="page-inner">
                <div class="sections">
                    <div class="section">
                        <header class="section__header">
                            <h2>General settings</h2>
                        </header>

                        <div class="section__inner">
                            <div class="section__content">
                                <p>It's recommended to keep this, but be free to change if you want.</p>
                            </div>

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
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted, computed } from "vue";
import yaml from "js-yaml";

const config = ref({
    backend: { name: "git-gateway", branch: "main" },
    media_folder: "public/upload",
    public_folder: "/upload"
});
</script>