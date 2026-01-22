---
name: macos-software-recommender
description: Recommend macOS software applications based on user needs, workflows, and categories. Access curated software listings from the awesome-mac ecosystem.
---

# macos-software-recommender

## Overview

This skill helps users discover and recommend macOS software applications based on their specific needs, workflows, and use cases. It leverages the comprehensive awesome-mac repository ecosystem to provide curated software recommendations across multiple categories.

## When to Use This Skill

Invoke this skill when users:
- Ask for macOS app recommendations ("What's the best note-taking app for Mac?")
- Need software for specific workflows ("I need a tool for screen recording")
- Want alternatives to specific applications ("Is there a free alternative to Photoshop?")
- Seek category-based discovery ("Show me good developer tools for macOS")
- Compare software options ("What's the difference between Xcode and VS Code?")

## Software Categories

### Core Categories

**Productivity & Writing**
- Text Editors: Aurora Edit, CotEditor, Sublime Text, VS Code
- Note-taking: Obsidian, Notion, Bear, Apple Notes
- Office Suites: Microsoft 365, LibreOffice, OnlyOffice
- Markdown Tools: Typora, MacDown, MarkText

**Development Tools**
- IDEs: Xcode, Android Studio, JetBrains Fleet
- Developer Utilities: Postman, Insomnia, Charles Proxy
- CLI Tools: iTerm2, Warp, Fig
- Git Clients: GitHub Desktop, Fork, Sublime Merge

**Design & Creative**
- Design Tools: Figma, Sketch, Adobe Creative Suite
- Prototyping: Axure RP, ProtoPie
- 3D Modeling: Blender, Cinema 4D
- Image Editing: Pixelmator Pro, Affinity Photo

**Communication**
- Collaboration: Slack, Discord, Microsoft Teams
- Email Clients: Spark, Mailplane, Airmail
- File Sharing: LocalSend, FileZilla, Cyberduck

**Utilities**
- Menu Bar Tools: Bartender, CleanMyMac, iStat Menus
- File Organization: Alfred, Raycast, Hazel
- Window Management: Rectangle, Magnet, BetterTouchTool
- Productivity: Alfred, Raycast, Hazel

**Audio & Video**
- Video Players: IINA, VLC, Movist
- Music Players: Spotify, Apple Music, Vox
- Audio Editing: Audacity, Adobe Audition
- Screen Recording: CleanShot X, Kap, OBS Studio

**Browsers**
- Chrome-based: Arc, Chrome, Brave, Edge
- Safari-based: Orion, SigmaOS
- Privacy-focused: Tor Browser, LibreWolf

**Security & Privacy**
- Password Managers: 1Password, Bitwarden, Apple Keychain
- VPNs: Mullvad, ExpressVPN, NordVPN
- Encryption: VeraCrypt, Cryptomator

## License Types

Software entries may include license indicators:
- **Open-Source Software**: Free and open source
- **Freeware**: Free to use but closed source
- **App Store Software**: Available on Mac App Store
- **Awesome List**: Part of curated awesome lists

## Recommendation Guidelines

### Understanding User Needs

1. **Identify the Use Case**: What specific problem does the user need to solve?
2. **Consider the Workflow**: How will this fit into their existing workflow?
3. **Assess Technical Level**: Match recommendations to user's technical expertise
4. **Budget Awareness**: Consider free vs paid options
5. **Integration Needs**: Check compatibility with existing tools

### Structuring Recommendations

Provide recommendations in this format:

```markdown
## Category: [Category Name]

Based on your needs, here are my recommendations:

### Top Pick
- **[App Name]** - [Brief description of why it's the top choice]
  - License: [License type]
  - Key Features: [3-5 bullet points]
  - Best For: [Target user type]

### Alternative Options
- **[App Name]** - [Description]
  - License: [License type]
  - Best For: [Specific use case]

### Considerations
- [Any important notes about compatibility, pricing, or limitations]
```

### Comparison Framework

When users ask for comparisons:
1. **Feature Parity**: Compare core features side-by-side
2. **Learning Curve**: Assess ease of use
3. **Performance**: Consider resource usage
4. **Ecosystem**: Integration with other tools
5. **Cost**: Price comparison

## Common Workflow Patterns

### For Developers
- Terminal + IDE + Git client + API testing tool
- Example: iTerm2 + VS Code + GitHub Desktop + Postman

### For Writers
- Distraction-free editor + Note-taking + Reference manager
- Example: Ulysses + Notion + Zotero

### For Designers
- Design tool + Prototyping + Asset management
- Example: Figma + Sketch + CleanShot X

### For Productivity
- Launcher + Window manager + Clipboard manager + Automation
- Example: Raycast + Rectangle + Maccy + Keyboard Maestro

## Research Process

When searching for recommendations:

1. **Query Formulation**: Use specific category and feature keywords
2. **Multiple Options**: Provide 2-4 alternatives per category
3. **License Diversity**: Include both free and paid options
4. **Recent Updates**: Prioritize actively maintained software
5. **Community Feedback**: Consider user ratings and reviews

## Limitations

- Software availability may vary by region
- App Store versions may differ from direct downloads
- Some software may require macOS version compatibility
- Pricing and features subject to change
- Not all software may be actively maintained

## Output Language Rule

**IMPORTANT**: All recommendations and responses from this skill MUST be output in **Chinese (Simplified)** unless the user explicitly requests another language.

## Real-time Data Fetching

**CRITICAL**: To ensure information accuracy and timeliness, ALWAYS perform the following steps before providing recommendations:

1. **Fetch Latest Data**: Use WebFetch or WebSearch to access the awesome-mac repository at `https://github.com/jaywcjlove/awesome-mac` or its README
2. **Verify Current Information**: Cross-check software names, descriptions, and license types against the fetched data
3. **Prioritize Active Projects**: Favor software that shows recent updates and active maintenance

This ensures recommendations are based on the most current software ecosystem rather than static cached information.

## Best Practices

1. **Ask Clarifying Questions**: When user needs are vague, ask about:
   - Specific use cases or workflows
   - Budget constraints
   - Technical expertise level
   - Integration requirements
   - Must-have vs nice-to-have features

2. **Provide Context**: Explain why each recommendation fits their needs

3. **Mention Alternatives**: Always provide backup options

4. **Stay Current**: Software landscape changes rapidly; prioritize recently updated apps

5. **Respect Preferences**: Honor user's stated preferences (open-source, App Store, etc.)

## Example Interactions

### Example 1: General Recommendation
**User**: "What's a good note-taking app for Mac?"

**Response**:
```markdown
Based on general note-taking needs, here are my top recommendations:

### Top Pick: Obsidian
- **Why**: Extremely flexible, markdown-based, local-first storage
- **License**: Free (personal use), paid sync available
- **Best For**: Power users who want control over their notes

### Alternative Options
- **Notion**: All-in-one workspace with databases and collaboration
- **Bear**: Beautiful, simple markdown editor with excellent iOS sync
- **Apple Notes**: Free, built-in, great for casual users

### Choosing Guide
- Choose Obsidian if you want customization and local storage
- Choose Notion for team collaboration and databases
- Choose Bear for simplicity and design
- Choose Apple Notes for zero-setup convenience
```

### Example 2: Specific Workflow
**User**: "I need tools for web development"

**Response**:
```markdown
Here's a complete web development toolkit for macOS:

### Core Development
- **VS Code** - Feature-rich extensible editor (Free)
- **iTerm2** - Powerful terminal replacement (Free)
- **GitHub Desktop** - Git GUI client (Free)

### API & Testing
- **Postman** - API development and testing (Free)
- **Charles Proxy** - HTTP debugging (Paid)

### Browser DevTools
- **Arc** - Modern browser with excellent dev tools (Free)
- **Chrome** - Standard for web development (Free)

This setup covers editing, terminal operations, version control, API testing, and browser debugging.
```

### Example 3: Alternative Request
**User**: "Free alternative to Photoshop?"

**Response**:
```markdown
Here are free alternatives to Adobe Photoshop for macOS:

### Top Pick: Photopea
- **Why**: Browser-based PS clone with similar interface
- **License**: Free (ad-supported) or paid
- **Capability**: Supports PSD files and most PS features

### Desktop Options
- **GIMP** - Full-featured open-source image editor
- **Krita** - Specialized for digital art and painting
- **Photopea** - Runs in browser, no installation needed

### Considerations
- Photopea requires internet connection
- GIMP has steeper learning curve
- Krita is better for illustration than photo editing
```

## Continuous Learning

The macOS software ecosystem evolves constantly. When providing recommendations:
- Check software update recency
- Verify macOS version compatibility
- Consider Apple Silicon (M1/M2/M3) optimization
- Note any major UI/UX changes in recent versions
