<template>
  <div id="app">
    <header class="header">
      <h1>PR UD 2</h1>
    </header>

    <div class="main-container">
      <!-- Storage Selector -->
      <aside class="sidebar">
        <ul>
          <li :class="{ active: selectedStorage === 'class' }" @click="selectStorage('class')">
            Class Storage
          </li>
          <li :class="{ active: selectedStorage === 'json' }" @click="selectStorage('json')">
            JSON
          </li>
          <li :class="{ active: selectedStorage === 'csv' }" @click="selectStorage('csv')">
            CSV
          </li>
        </ul>
      </aside>

      <!-- Content Section -->
      <section class="content">
        <div class="actions-grid">
          <button @click="getFiles" class="action-btn">Get Files</button>
          <button @click="prepareStoreFile" class="action-btn">Store</button>
          <button @click="prepareShowFile" class="action-btn">Show</button>
          <button @click="prepareUpdateFile" class="action-btn">Update</button>
          <button @click="prepareDeleteFile" class="action-btn">Delete</button>
        </div>

        <!-- Inputs Section -->
        <div v-if="showInput" class="input-group">
          <input v-model="filename" placeholder="Enter filename" class="input-field" />
          <button @click="send" class="input-btn">Send</button>
        </div>

        <div v-if="storeInput" class="input-group">
          <input v-model="filename" placeholder="Enter filename" class="input-field" />
          <textarea v-model="fileContent" placeholder="Enter file content" class="textarea-field"></textarea>
          <button @click="storeFile" class="input-btn">Save</button>
        </div>

        <div v-if="updateInput" class="input-group">
          <input v-model="filename" placeholder="Enter filename" class="input-field" />
          <textarea v-model="fileContent" placeholder="Enter new content" class="textarea-field"></textarea>
          <button @click="updateFile" class="input-btn">Update</button>
        </div>

        <div v-if="deleteInput" class="input-group">
          <input v-model="filename" placeholder="Enter filename to delete" class="input-field" />
          <button @click="deleteFile" class="input-btn">Delete</button>
        </div>

        <!-- Result Section -->
        <div class="results">
          <h2>Results</h2>
          <pre>{{ result }}</pre>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      selectedStorage: "class",
      result: "",
      filename: "",
      fileContent: "",
      showInput: false,
      storeInput: false,
      updateInput: false,
      deleteInput: false,
    };
  },
  methods: {
    selectStorage(storage) {
      this.selectedStorage = storage;
      this.resetInputs();
    },
    resetInputs() {
      this.result = "";
      this.filename = "";
      this.fileContent = "";
      this.showInput = false;
      this.storeInput = false;
      this.updateInput = false;
      this.deleteInput = false;
    },
    getApiUrl(action = "", id = "") {
      const baseUrls = {
        class: "http://localhost:8000/api/hello",
        json: "http://localhost:8000/api/json",
        csv: "http://localhost:8000/api/csv",
      };
      return `${baseUrls[this.selectedStorage]}${id ? `/${id}` : ""}`;
    },
    async getFiles() {
      try {
        const url = this.getApiUrl();
        const response = await axios.get(url);
        this.result = JSON.stringify(response.data, null, 2);
      } catch (error) {
        this.result = `Error: ${error.response?.status} - ${error.response?.data?.mensaje || error.message}`;
      }
    },
    prepareShowFile() {
      this.resetInputs();
      this.showInput = true;
    },
    async send() {
      try {
        if (!this.filename) {
          this.result = "Please provide a filename.";
          return;
        }
        const url = this.getApiUrl("", this.filename);
        const response = await axios.get(url);
        this.result = JSON.stringify(response.data, null, 2);
      } catch (error) {
        this.result = `Error: ${error.response?.status} - ${error.response?.data?.mensaje || error.message}`;
      }
    },
    prepareStoreFile() {
      this.resetInputs();
      this.storeInput = true;
    },
    async storeFile() {
      try {
        if (!this.filename || !this.fileContent) {
          this.result = "Please provide a filename and content.";
          return;
        }
        const url = this.getApiUrl();
        const response = await axios.post(url, {
          filename: this.filename,
          content: this.fileContent,
        });
        this.result = JSON.stringify(response.data, null, 2);
      } catch (error) {
        this.result = `Error: ${error.response?.status} - ${error.response?.data?.mensaje || error.message}`;
      }
    },
    prepareUpdateFile() {
      this.resetInputs();
      this.updateInput = true;
    },
    async updateFile() {
      try {
        if (!this.filename || !this.fileContent) {
          this.result = "Please provide a filename and content.";
          return;
        }
        const url = this.getApiUrl("", this.filename);
        const response = await axios.put(url, { content: this.fileContent });
        this.result = JSON.stringify(response.data, null, 2);
      } catch (error) {
        this.result = `Error: ${error.response?.status} - ${error.response?.data?.mensaje || error.message}`;
      }
    },
    prepareDeleteFile() {
      this.resetInputs();
      this.deleteInput = true;
    },
    async deleteFile() {
      try {
        if (!this.filename) {
          this.result = "Please provide a filename.";
          return;
        }
        const url = this.getApiUrl("", this.filename);
        const response = await axios.delete(url);
        this.result = JSON.stringify(response.data, null, 2);
      } catch (error) {
        this.result = `Error: ${error.response?.status} - ${error.response?.data?.mensaje || error.message}`;
      }
    },
  },
};
</script>

<style scoped>
body {
  font-family: Arial, sans-serif;
  background-color: #f9fafc;
  margin: 0;
  padding: 0;
}
.header {
  background: #333;
  color: #fff;
  padding: 1em;
  text-align: center;
  font-size: 24px;
  font-weight: bold;
}
.main-container {
  display: flex;
  height: calc(100vh - 100px);
  padding: 1em;
  gap: 1em;
}
.sidebar {
  width: 200px;
  background: #ffffff;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  padding: 10px;
}
.sidebar ul {
  list-style: none;
  padding: 0;
}
.sidebar li {
  padding: 10px;
  margin: 5px 0;
  text-align: center;
  cursor: pointer;
  transition: all 0.3s ease;
  border-radius: 6px;
  font-weight: bold;
}
.sidebar li.active,
.sidebar li:hover {
  background: #007bff;
  color: #fff;
}
.content {
  flex: 1;
  padding: 20px;
  background: #ffffff;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
}
.actions-grid {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}
.action-btn {
  background: #007bff;
  color: #fff;
  border: none;
  padding: 10px 20px;
  border-radius: 6px;
  cursor: pointer;
  font-size: 14px;
  transition: all 0.3s ease;
}
.action-btn:hover {
  background: #0056b3;
}
.input-group {
  margin-top: 20px;
}
.input-field,
.textarea-field {
  width: 100%;
  margin-bottom: 10px;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 6px;
  font-size: 14px;
}
.input-btn {
  background: #28a745;
  color: #fff;
  border: none;
  padding: 10px 20px;
  border-radius: 6px;
  cursor: pointer;
  font-size: 14px;
}
.input-btn:hover {
  background: #218838;
}
.results {
  margin-top: 20px;
  background: #f8f9fa;
  padding: 20px;
  border-radius: 6px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}
.results pre {
  font-size: 14px;
  color: #333;
}
</style>
