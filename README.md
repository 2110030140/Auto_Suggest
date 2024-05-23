Trie Auto-Suggest

About This Project

This project implements a simple Trie data structure to provide auto-suggest functionalities using HTML, CSS, and JavaScript. The web application allows users to insert words into the Trie and retrieve word suggestions based on a given prefix.

Project Description

Features:

- Insert Word: Users can add new words to the Trie.
- Auto-Suggest: Users can get a list of word suggestions based on a prefix.

Trie Data Structure

A Trie (pronounced "try") is a tree-like data structure that is used to efficiently store and retrieve keys in a dataset of strings. Here are the key characteristics of a Trie:

1. Nodes and Edges: Each node represents a character of a string, and each edge connects two nodes, representing the transition from one character to another.
2. Root: The root node is an empty node that doesn't hold any character.
3. End of Word (EOW): A flag at each node indicates whether it represents the end of a valid word.

The Trie is particularly useful for autocomplete and spell-checking applications because it allows fast insertion and lookup operations.

Prerequisites

To run this project, you need a modern web browser (such as Chrome, Firefox, or Edge) that supports JavaScript.

Running the Application

1. Clone the Repository:
   git clone https://github.com/your-username/trie-auto-suggest.git

2. Navigate to the Project Directory:
   cd trie-auto-suggest

3. Open the `index.html` File in Your Browser:
   - You can simply double-click the `index.html` file, or open it through your browser's "Open File" option.

Code Overview

HTML

The HTML file sets up the basic structure of the web page, including a container for user inputs and a section to display the suggestions.

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Trie Auto-Suggest</title>
    <style>
        /* Add CSS styles here */
    </style>
</head>
<body>
    <div class="container">
        <h1>Trie Auto-Suggest</h1>
        <div class="input-container">
            <label for="word-input">Enter a word to insert:</label>
            <input type="text" id="word-input">
            <button onclick="insertWord()">Insert</button>
        </div>
        <div class="input-container">
            <label for="prefix-input">Enter a prefix for suggestions:</label>
            <input type="text" id="prefix-input">
            <button onclick="getSuggestions()">Suggest</button>
        </div>
        <div id="suggestions"></div>
    </div>
    <script>
        // JavaScript code goes here
    </script>
</body>
</html>


CSS

The CSS styles define the layout and appearance of the web page.

body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    background-image: url('background-image.jpg');
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}
.container {
    background-color: rgba(255, 255, 255, 0.9);
    padding: 30px;
    border-radius: 10px;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.3);
    max-width: 600px;
    width: 80%;
}
h1 {
    color: #333;
    text-align: center;
    margin-bottom: 30px;
}
label {
    display: block;
    margin-bottom: 10px;
    font-weight: bold;
    color: #555;
}
input[type="text"] {
    width: calc(70% - 12px);
    padding: 10px;
    margin-bottom: 10px;
    border: 1px solid #ccc;
    border-radius: 3px;
}
button {
    width: calc(30% - 12px);
    padding: 10px 20px;
    border: none;
    border-radius: 3px;
    background-color: #007BFF;
    color: white;
    cursor: pointer;
    transition: background-color 0.3s ease;
}
button:hover {
    background-color: #0056b3;
}
#suggestions {
    margin-top: 20px;
}
#suggestions h2 {
    margin: 0 0 10px 0;
    font-size: 1.2em;
    color: #333;
}
#suggestions ul {
    list-style-type: none;
    padding: 0;
    margin: 0;
}
#suggestions li {
    margin: 5px 0;
    padding: 10px;
    background-color: #f9f9f9;
    border: 1px solid #ccc;
    border-radius: 3px;
    box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
}
.input-container {
    display: flex;
    margin-bottom: 15px;
}
.input-container label {
    flex: 1;
    margin-right: 10px;
}
.input-container input {
    flex: 3;
}
.input-container button {
    flex: 1;
    width: auto;
}
JavaScript

The JavaScript code implements the Trie data structure and its functionalities.

Trie Node

A Trie node (TNode) represents each character in the Trie. Each node contains an array of children nodes and a boolean flag indicating if the node is the end of a word (EOW).

class TNode {
    constructor() {
        this.isEOW = false;
        this.children = new Array(26).fill(null);
    }
}

Trie Class

The Trie class manages the root node and provides methods for inserting words and retrieving suggestions.

javascript
class Trie {
    constructor() {
        this.root = new TNode();
    }

    insertWord(word) {
        let node = this.root;
        for (const char of word) {
            const index = char.charCodeAt(0) - 'a'.charCodeAt(0);
            if (!node.children[index]) {
                node.children[index] = new TNode();
            }
            node = node.children[index];
        }
        node.isEOW = true;
    }

    autosuggest(prefix) {
        let node = this.root;
        for (const char of prefix) {
            const index = char.charCodeAt(0) - 'a'.charCodeAt(0);
            if (!node.children[index]) {
                // Prefix not found
                return [];
            }
            node = node.children[index];
        }
        const suggestions = [];
        this.autosuggestHelper(node, prefix, suggestions);
        return suggestions;
    }

    autosuggestHelper(node, currentPrefix, suggestions) {
        if (node.isEOW) {
            suggestions.push(currentPrefix);
        }
        for (let i = 0; i < 26; i++) {
            if (node.children[i]) {
                this.autosuggestHelper(node.children[i], currentPrefix + String.fromCharCode(i + 'a'.charCodeAt(0)), suggestions);
            }
        }
    }
}

Initializing the Trie

We initialize the Trie and insert some initial words for demonstration purposes.

javascript
const trie = new Trie();

// Insert initial words
trie.insertWord("apple");
trie.insertWord("zebra");
trie.insertWord("app");
trie.insertWord("jj");

Inserting Words

The `insertWord` function is triggered by the user input to add words to the Trie.

javascript
function insertWord() {
    const wordInput = document.getElementById('word-input').value;
    if (wordInput) {
        trie.insertWord(wordInput.toLowerCase());
        document.getElementById('word-input').value = '';
        alert(`Inserted: ${wordInput}`);
    }
}

Getting Suggestions

The `getSuggestions` function retrieves suggestions based on the prefix entered by the user.

javascript
function getSuggestions() {
    const prefixInput = document.getElementById('prefix-input').value;
    const suggestions = trie.autosuggest(prefixInput.toLowerCase());
    const suggestionsDiv = document.getElementById('suggestions');
    suggestionsDiv.innerHTML = '<h2>Suggestions:</h2>';
    const ul = document.createElement('ul');
    suggestions.forEach(suggestion => {
        const li = document.createElement('li');
        li.textContent = suggestion;
        ul.appendChild(li);
    });
    suggestionsDiv.appendChild(ul);
}

Conclusion

You have now set up a Trie-based web application with functionalities for inserting words and suggesting words based on a prefix. This README file provides an overview of the project and instructions for running it.

