# Folder Structure
- The vault follows a fullstack hierarchical folder structure where everything is grouped in folders, and each folder has an index file used to reference every file or nested folder inside
# Tag System
- The Vault uses a hierarchical tag system to compliment the folder structure and link everything together
- Tags are thought of as an organization where:
	- There are top level tags (departments) such as (structure is subject to change)
		1. Type
		2. Topic
		3. Context
			- **Target context:** What you're testing
			- **Phase context:** Where in methodology
			- **Finding context:** What type of issue
			- **Technical context:** Ports, services, etc.
		4. Tool
		5. Category
	- 2nd level tags (teams) (also subject to change)
		1. Phase(s)
		2. Scan
		3. (any tool name under tool)
		4. Vulnerability
		5. Target
		6. Findings
		7. Reference
		8. CLI
		9. Practice
		10. Domain
	- 3rd and usually final level
		- These are usually very specific things like file/folder names, a challenge level, hacking phase, and so on
	- Naming is always lower case, and words separated by hyphens
	- Most tags are optional, but some should exist in every file
		- One type/ tag (what kind of content)
		- One+ topic/ tags (what it's about)
		- Optional context/ tags (situational info)
		- Index files are an example as they instead need
			- Type
			- Domain
	-  When dealing with pentesting assessments remember that each file should have
		-  **What it is:** `type/findings`
		- **Where found:** `context/target/klioptrix/level/1`
		- **When found:** `context/phase/reconnaissance`
		- **What type:** `context/vulnerability/[specific-type]`
	- Tags explained:
		- topic: File content type. E.g, notes, index, etc...
		- area: root level folder
		- domain: first level folder, under root
		- topic: 2nd level folder, under domain, more specific
		- category: Supplements type, and can exist across different areas. E.g, daily
		- context: Differentiates between similar types
			- area/work, domain/projects, context/compass or context/myfleeta
			- context is used on every file belonging to the context including the index