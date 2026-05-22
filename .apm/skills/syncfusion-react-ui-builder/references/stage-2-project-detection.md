# Stage 2: Project Detection

**Purpose:** Auto-detect project structure, framework, language, and configuration to ensure generated code integrates seamlessly.

**AI Should Auto-Detect:**

1. **Framework Type**
   - Scan for: `package.json`, `next.config.js`, `nuxt.config.js`, `vite.config.ts`, etc.
   - Detect: React only, Next.js (App Router vs Pages Router), Vite, Create React App
   
2. **Language Preference**
   - Check for: `tsconfig.json` → TypeScript enabled
   - Check for: `.js` vs `.ts` files in src/
   - Default: TypeScript if tsconfig.json exists
   
3. **CSS Strategy**
   - Check for: `tailwind.config.js` → Tailwind CSS
   - Check for: CSS Modules in imports
   - Default: CSS Modules
   
4. **Component Directory**
   - Common paths: `src/components/`, `app/components/`, `components/`
   - Find existing component patterns
   
5. **Formatting Rules**
   - Read `.prettierrc` or `prettier.config.js` for indent, quotes, semicolons
   - Read `.eslintrc` for linting rules
   - Apply same rules to generated code
   
6. **Syncfusion License & Package Versioning**
   - Check: Is `SYNCFUSION_LICENSE_KEY` in `.env.local`?
   - Check: Does project already call `registerLicense()`?
   - Prompt: If missing, ask user for license key
   
7. **Syncfusion Package Version Detection**
   - **scan `package.json` for existing Syncfusion packages:**
      - If `@syncfusion/ej2-react-*` exists: Extract the full version (e.g., `33.2.5`) and take the major number only (e.g., `33`)
      - Use the major version for all new Syncfusion package installs → Avoids patch-not-found errors: `npm install @syncfusion/ej2-react-schedule@33`
      - Never mix major versions — all Syncfusion packages must share the same major number
   - **If NO existing Syncfusion packages found:**
      - Use * (latest) version for all new packages: `@syncfusion/ej2-react-grids@*`
      - Document version decision: Log detected version in stage output
   - **Document version decision:** Log detected version in stage output
   
**Output:**
Show detected settings:
```
✓ Framework: Next.js 14 (App Router)
✓ Language: TypeScript (strict mode)
✓ CSS: CSS Modules
✓ Component Dir: app/components/
✓ Formatting: 2 spaces, no semicolons
✓ Syncfusion Version: 23.1.36 (detected from package.json)
  OR
✓ Syncfusion Version: * (latest - no existing packages)

```
