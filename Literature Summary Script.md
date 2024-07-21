```dataviewjs
// DataviewJS script to create a table with File, Author, Title, Year, Summary, Hypothesis, Methodology, Results, and Significance from files in the "Literature" folder

// Define the folder to search
const folder = "Literature";

// Function to generate Markdown table content
function generateMarkdownTable(data) {
    const headers = ["File", "Author", "Title", "Year", "Summary", "Hypothesis", "Methodology", "Results", "Significance"];
    const separator = headers.map(() => "---").join(" | ");
    const headerRow = headers.join(" | ");
    const rows = data.map(row => row.map(cell => String(cell).replace(/\n/g, "<br>")).join(" | "));
    return [headerRow, separator, ...rows].join("\n");
}

// Function to escape special characters in Markdown links
function escapeMarkdownLink(text) {
    return text.replace(/([\\`*_\[\]{}()#+\-.!])/g, '\\$1');
}

// Function to encode spaces in file paths
function encodeSpaces(text) {
    return text.replace(/ /g, '%20');
}

// Create the table header and collect data
const data = await Promise.all(dv.pages(`"${folder}"`).map(async page => {
    // Extract the author, title, and year from the frontmatter
    const author = page.author || "";
    const title = page.title || "";
    const year = page.year || "";
    
    // Initialize content for each section
    let summary = "";
    let hypothesis = "";
    let methodology = "";
    let results = "";
    let significance = "";

    // Load the file content
    const content = await dv.io.load(page.file.path);
    if (content) {
        // Extract content under each heading
        const lines = content.split("\n");
        let currentSection = null;
        for (const line of lines) {
            if (line.startsWith("### Summary")) {
                currentSection = "summary";
                continue;
            } else if (line.startsWith("### Hypothesis")) {
                currentSection = "hypothesis";
                continue;
            } else if (line.startsWith("### Methodology")) {
                currentSection = "methodology";
                continue;
            } else if (line.startsWith("### Result")) {
                currentSection = "results";
                continue;
            } else if (line.startsWith("### Significance")) {
                currentSection = "significance";
                continue;
            } else if (line.startsWith("# ") || line.startsWith("## ") || line.startsWith("### ") || line.startsWith("#### ") || line.startsWith("##### ") || line.startsWith("###### ")) {
                currentSection = null;
            }

            if (currentSection) {
                if (currentSection === "summary") {
                    summary += line + "\n";
                } else if (currentSection === "hypothesis") {
                    hypothesis += line + "\n";
                } else if (currentSection === "methodology") {
                    methodology += line + "\n";
                } else if (currentSection === "results") {
                    results += line + "\n";
                } else if (currentSection === "significance") {
                    significance += line + "\n";
                }
            }
        }
    }

    // Create a link to the original file
    const fileLink = `[${escapeMarkdownLink(page.file.name)}](${encodeSpaces(escapeMarkdownLink(page.file.path))})`;


    // Return the file link, author, title, year, and content for each section
    return [fileLink, author, title, year, summary.trim(), hypothesis.trim(), methodology.trim(), results.trim(), significance.trim()];
}));

// Generate the Markdown table content
const markdownTable = generateMarkdownTable(data);

// Define the file path and content
const filePath = "Literature Dataviews/Literature Summary Table.md";
const fileContent = `${markdownTable}`;

// Check if the file already exists
const existingFile = app.vault.getAbstractFileByPath(filePath);
if (existingFile) {
    // If the file exists, overwrite it
    await app.vault.modify(existingFile, fileContent);
} else {
    // If the file does not exist, create it
    await app.vault.create(filePath, fileContent);
}

// Display the table in the current note
dv.table(["File", "Author", "Title", "Year", "Summary", "Hypothesis", "Methodology", "Results", "Significance"], data);

```

