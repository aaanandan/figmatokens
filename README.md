# Figma Design Token Generator

A Next.js application that automates the process of fetching Figma variables, transforming them into design tokens, and committing them to your repository.

## Features

- 🎨 **Fetch Figma Variables** - Pull design variables directly from your Figma file
- 🔄 **Transform to DTCG Format** - Convert Figma variables to Design Tokens Community Group (DTCG) format
- 🏗️ **Multi-Platform Build** - Generate tokens for:
  - CSS (CSS variables)
  - JavaScript (ES6 modules)
  - iOS (Swift)
  - Android (XML resources)
- 🚀 **Git Integration** - Automatically commit and push generated tokens to your repository
- 📊 **Real-time Progress** - Track each step of the generation process with visual feedback

## Architecture

```
User Input (Figma API Key, File ID)
         ↓
  Fetch Variables from Figma API
         ↓
  Transform to DTCG Tokens
         ↓
  Build with Style Dictionary
         ↓
  Commit to Git Repository
```

## Getting Started

### Prerequisites

- Node.js 18+ 
- npm or yarn
- Figma Personal Access Token
- Git repository (for automatic commits)

### Installation

1. Clone this repository:
```bash
git clone <your-repo-url>
cd figma-token-generator
```

2. Install dependencies:
```bash
npm install
```

3. Configure environment variables:
```bash
cp .env.example .env
```

Edit `.env` with your Git repository details:
```env
GIT_REPO_URL=https://github.com/your-org/your-repo.git
GIT_BRANCH=main
GIT_USERNAME=your-github-username
GIT_EMAIL=your-email@example.com
GIT_TOKEN=your-personal-access-token
GIT_COMMIT_PREFIX="chore: update design tokens"
```

### Running the Application

Development mode:
```bash
npm run dev
```

Production build:
```bash
npm run build
npm start
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

## Usage

1. **Enter Figma API Token**
   - Go to Figma → Settings → Account → Personal Access Tokens
   - Generate a new token with file read permissions
   - Paste it in the form

2. **Enter Figma File Key**
   - Open your Figma file
   - Copy the file key from the URL: `figma.com/file/{FILE_KEY}/...`
   - Paste it in the form

3. **Add Commit Message (Optional)**
   - Provide context for this token update
   - Example: "Updated brand colors for dark mode"

4. **Click Generate Tokens**
   - Watch the progress as each step completes
   - View success/error messages for each step

## Output Structure

After generation, you'll have:

```
tokens/
  ├── figma-variables.json      # Raw Figma API response
  └── design-tokens.dtcg.json   # Transformed DTCG tokens

build/
  ├── css/
  │   └── tokens.css            # CSS custom properties
  ├── js/
  │   └── tokens.js             # JavaScript ES6 module
  ├── ios/
  │   └── DesignTokens.swift    # iOS Swift class
  └── android/
      └── tokens.xml            # Android XML resources
```

## Git Configuration

The application can automatically commit generated tokens to your repository. This requires:

1. A GitHub Personal Access Token with `repo` scope
2. Repository URL (HTTPS or SSH)
3. Git credentials (username and email)

If Git is not configured, the application will skip the commit step but still generate the tokens locally.

## API Reference

### POST `/api/generate-tokens`

Generate design tokens from Figma variables.

**Request Body:**
```json
{
  "figmaToken": "figd_...",
  "figmaFileKey": "abc123xyz",
  "commitMessage": "Updated colors" // optional
}
```

**Response:**
```json
{
  "success": true,
  "message": "Successfully generated and committed design tokens",
  "steps": [
    {
      "step": "fetch",
      "status": "success",
      "message": "Successfully fetched Figma variables",
      "timestamp": "2025-01-15T10:30:00Z"
    },
    // ... more steps
  ]
}
```

## Project Structure

```
figma-token-generator/
├── app/
│   ├── api/
│   │   └── generate-tokens/
│   │       └── route.ts          # API endpoint
│   ├── page.tsx                   # Main page
│   ├── layout.tsx                 # Root layout
│   └── globals.css                # Global styles
├── components/
│   └── TokenGeneratorForm.tsx     # Main form component
├── lib/
│   ├── figma-client.ts            # Figma API integration
│   ├── token-processor.ts         # Token transformation
│   ├── style-dictionary-builder.ts # Style Dictionary build
│   ├── git-integration.ts         # Git operations
│   └── types.ts                   # TypeScript types
├── tokens/                        # Generated token files
├── build/                         # Built platform files
├── package.json
├── tsconfig.json
└── next.config.js
```

## Customization

### Modify Token Output

Edit `lib/style-dictionary-builder.ts` to:
- Add new platforms
- Change file formats
- Customize transformations

### Change Token Structure

Edit `lib/token-processor.ts` to:
- Adjust naming conventions
- Add custom token types
- Modify value transformations

## Troubleshooting

### Figma API Errors

- **401 Unauthorized**: Check your Figma API token
- **404 Not Found**: Verify the file key is correct
- **Rate Limit**: Wait before making more requests

### Git Errors

- **Authentication Failed**: Verify your Git token has correct permissions
- **Push Rejected**: Ensure you have write access to the repository
- **Branch Not Found**: Check the branch name in your `.env`

### Build Errors

- **Module Not Found**: Run `npm install` again
- **Type Errors**: Check TypeScript version compatibility

## Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## License

MIT

## Acknowledgments

- [Figma Variables API](https://www.figma.com/developers/api#variables)
- [Style Dictionary](https://amzn.github.io/style-dictionary/)
- [DTCG Format Specification](https://tr.designtokens.org/format/)
