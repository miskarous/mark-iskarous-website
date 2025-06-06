# Project Handover Document

## Overview

This document provides an overview of the Mark Iskarous website codebase, its architecture, and development workflows. The website is built using modern web technologies and follows current best practices.

## Contact

For any questions or issues, please contact [Bailey Kane](https://baileykane.co/).

## Technology Stack

### Core Technologies

- **Astro**: A modern static site builder that allows you to build fast, content-focused websites. It's the main framework of this project.
- **React**: Used for interactive components within the Astro framework.
- **TypeScript**: Provides type safety and better developer experience.
- **Tailwind CSS**: A utility-first CSS framework for styling.
- **Sanity.io**: A headless CMS (Content Management System) used for content management.

### Key Dependencies

- `@astrojs/react`: Integration between Astro and React
- `@astrojs/tailwind`: Tailwind CSS integration for Astro
- `@astrojs/vercel`: Deployment configuration for Vercel
- `sanity`: Sanity.io client and tools
- `swiper`: For carousel/slider components
- `lucide-react`: Icon library
- `tailwind-merge`: Utility for merging Tailwind classes

## Project Structure

```
src/
├── components/     # Reusable UI components
├── layouts/        # Page layout templates
├── pages/          # Astro pages and routes
├── sanity/         # Sanity.io configuration and schemas
└── styles/         # Global styles and Tailwind configuration
```

## Development Workflow

### Setting Up the Development Environment

1. **Prerequisites**

   - Node.js (latest LTS version recommended)
   - `npm` or `yarn` package manager
   - Git

2. **Installation**

   ```bash
   npm install
   ```
   Run this command in the project root to install the project dependencies through the chosen package manager. This is only required once.

3. **Environment Variables**

   Set up your local environment with the required environment variables. These are sensitive pieces of data that components of the project use for things like API access or tool configuration.

   In our case, these are used to configure the Sanity Studio CMS access.

   Create a `.env.local` file in the project root and add the required environment variables for Sanity Studio. If lost, these can be found in your Sanity account.

   *Note: These environment variables are already configured in the Vercel project.*

5. **Development Server**
   ```bash
   npm run dev
   ```
   This starts a local development server, by default at `http://localhost:4321`

### Common Development Tasks

1. **Adding New Pages**

   - Create new `.astro` files in the `src/pages` directory
   - The file name determines the URL route (e.g., `pages/about.astro` → `/about`)

2. **Creating Components**

   - Reusable components go in `src/components`
   - Follow the existing component structure
   - Components can be made with Astro or a Javascript framework like React

3. **Styling**

   - Use [Tailwind CSS](https://v3.tailwindcss.com/) classes directly in components
   - Note: Uses **Tailwind V3**, so make sure you view the correct documentation to learn more. Or, upgrade to the latest Tailwind version.
   - Global styles can be added in `src/styles`

4. **Content Management**

   - Content is managed through Sanity.io
   - The schema definitions are in `src/sanity`
   - Use the Sanity Studio interface for content updates

5. **Adding Navigation Items**

   To add a new item to the navigation bar:

   1. Open `src/components/global/NavBar.astro`
   2. Add a new `NavBarItem` component in the navigation links section:
      ```astro
      <div
        class="flex md:justify-end flex-col md:flex-row items-center gap-4 mt-4 md:mt-0 fixed md:relative inset-x-0 top-16 md:top-0 bg-neutral-100 md:bg-transparent z-50 py-4 md:py-0"
      >
        <NavBarItem location="/" title="Home" />
        <NavBarItem location="/research" title="Research" />
        <NavBarItem location="/teaching" title="Teaching" />
        <NavBarItem location="/publications" title="Publications" />
        <NavBarItem location={`${cv.fileUrl}`} title="CV" target="_blank" />
        <!-- Add your new navigation item here -->
        <NavBarItem location="/your-new-page" title="New Page" />
      </div>
      ```

   The `NavBarItem` component takes three props:

   - `location`: The URL path for the navigation item
   - `title`: The text to display in the navigation bar
   - `target` (optional): Set to `"_blank"` to open the link in a new tab (used for external links like the CV)

   The navigation bar is responsive and will automatically handle mobile views with a hamburger menu.

### `npm` commands

There are pre-configured `npm` commands that are used to interact with the code base.

- `npm run dev` - This starts a development server, by default located at `localhost:4321`. This is used to run a local version of the site for development and testing.
- `npm run build` - This generates a production-ready build of the project. This is what Vercel will run when pushing code to GitHub / Vercel, and can be run locally to ensure the project builds correctly beforehand.

### Deployment

The site is deployed on Vercel. The deployment process is automated:

1. Push changes to the main branch
2. Vercel automatically builds and deploys the changes
3. Preview deployments are created for pull requests

## Key Concepts for New Developers

### Astro

- Astro is a static site generator that allows you to write components in multiple frameworks
- Pages are written in `.astro` files, which use a template syntax similar to HTML
- Components can be written in React, but they're rendered to static HTML at build time

### Sanity.io

- Sanity is a headless CMS that manages all the website's content
- Content is structured using schemas defined in `src/sanity`
- The website fetches content from Sanity at build time

### TypeScript

- All components and utilities are written in TypeScript
- Type definitions help catch errors early and provide better IDE support
- The `tsconfig.json` file defines TypeScript configuration

### Tailwind CSS

- Utility-first CSS framework
- Classes are used directly in HTML/JSX
- Configuration is in `tailwind.config.mjs`

## Common Issues and Solutions

1. **Build Errors**

   - Check TypeScript errors first
   - Ensure all required environment variables are set
   - Verify Sanity.io connection

2. **Styling Issues**

   - Use the browser's dev tools to inspect elements
   - Check for conflicting Tailwind classes
   - Verify the component's responsive design

3. **Content Updates**
   - Use Sanity Studio for content changes
   - Verify content structure matches the schema
   - Check for missing required fields

## Resources

- [Astro Documentation](https://docs.astro.build)
- [Sanity.io Documentation](https://www.sanity.io/docs)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)
- [React Documentation](https://react.dev)
- [TypeScript Documentation](https://www.typescriptlang.org/docs)

## Infrastructure Components

The project is built on several key infrastructure components that work together:

### 1. Sanity Schema Types (`src/sanity/schemaTypes/`)

- Defines the data structure and validation rules for all content
- Each content type has its own file (e.g., `publication.ts`, `researchTopic.ts`)
- Types are defined using Sanity's schema language
- All types must be imported and exported in `schemaTypes/index.ts`
- Example types include:
  - `publication.ts`: Publications
  - `researchTopic.ts`: Research topics
  - `teachingPost.ts`: Teaching posts
  - `homePage.ts`: Home page content structure

### 2. Sanity Studio

- Content management interface
- Automatically generated from the schema types
- Allows content editors to create and manage content
- Accessible at `/admin`. Sometimes inaccessible on the development server

### 3. Astro Pages and Components

- Pages are defined in `src/pages/`
- Components are defined in `src/components/`
- Uses [GROQ (Graph-Relational Object Queries)](https://www.sanity.io/docs/groq-reference) to fetch content from Sanity
- Combines content with UI components and styling
- Example query structure:
  ```typescript
  const query = `*[_type == "publication"]{
    title,
    taglineBlock,
    abstract
    // ... other fields
  }`;
  ```

### 4. TailwindCSS

- Defines the visual styling of components
- Configuration in `tailwind.config.mjs`
- Uses utility classes directly in components
- Supports responsive design through breakpoint queries

## Making Content and Representation Changes

Here's a step-by-step guide for making changes to content types and their representation:

### 1. Define or Edit a Data Type

1. Create a new file in `src/sanity/schemaTypes/` or edit an existing one

   ```typescript
   // Example: src/sanity/schemaTypes/newContent.ts
   export default {
     name: "newContent",
     title: "New Content",
     type: "document",
     fields: [
       {
         name: "title",
         title: "Title",
         type: "string",
         validation: (Rule) => Rule.required(),
       },
       // ... other fields
     ],
   };
   ```

2. Import and export the new type in `src/sanity/schemaTypes/index.ts`

   ```typescript
   import newContent from "./newContent";

   export const schemaTypes = [
     // ... existing types
     newContent,
   ];
   ```

### 2. Add Content in Sanity Studio

1. Start the development server:

   ```bash
   npm run dev
   ```

2. Navigate to `http://localhost:4321/admin`. If this doesn't work, push the content type changes and access Sanity Studio on the production domain

3. Create new content using the Sanity Studio interface
   - The interface will automatically reflect your new schema
   - Fill in all required fields
   - Save your changes

### 3. Display the Content

1. Create or modify a component to query the new content:

   ```typescript
   // Example component
   import { sanityClient } from "sanity:client";
   import type { SanityDocument } from "@sanity/client";

   const publicationCategories: Array<SanityDocument> =
     await sanityClient.fetch(`*[_type == "publicationCategory"]
         {_id, title, description, order}`);
   ```

2. Add the component to a page or layout:

   ```astro
   ---
   import NewContentComponent from "../components/NewContentComponent";
   ---

   <NewContentComponent />
   ```

3. Style the component using Tailwind classes:
   ```html
   <div class="max-w-2xl mx-auto p-4">
     <h1 class="text-2xl font-bold">{content.title}</h1>
     <!-- ... other content -->
   </div>
   ```

### Common Patterns

1. **Querying Content**

   - Use GROQ to query Sanity
   - Filter and sort content as needed
   - Handle loading and error states

2. **Component Structure**

   - Keep components focused and reusable
   - Use TypeScript for type safety
   - Follow existing component patterns

3. **Styling**
   - Use Tailwind utility classes
   - Maintain consistent spacing and typography
   - Ensure responsive design
