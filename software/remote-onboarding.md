I'm a big believer in People as the **most fundamental part of any startup**. In the current state of Remote work, the onboarding process is challenging and essential to the success of the new hires.

These are some of my thoughts to make this process smooth.

- Early access to communication tools
- **Onboarding buddy** (Someone to ask questions) 
- At least two full weeks for onboarding (Culture, Vision, Politics, Security, Courses, ...)
- **Job shadowing** (Learn from people with similar or the same role) 
- Asynchronous learning of the Domain (Its a plus to have this documented in Notion or Confluence)
- Template of **user access privileges** and tools to 
ensuring the work is carried out properly (Matrix of user privileges based on role)
- The third week, you must start to contribute to your team's work (Issues, low-risk tasks)
- Keep learning about the Culture and the Domain
- Make the onboarding document public within the company and encourage new hires to **contribute and make the onboarding process better**
- **Setup expectations** as an individual contributor and as a manager (Management style, 1:1 cadency, ...)
- Deep Dive into the code and principal abstractions and hot spots of the new company
- **KPIs awareness**, as creative workers it's valuable to know how to do a good job (it keeps people focused and motivated)
- Document the onboarding process
- Take time to learn, invest time in learning, don't be afraid to ask

**Note:**
In the case of MAC users, we make the process simple by sharing our [Homebrew](https://brew.sh) formulas, plus the use of [Sdkman](https://sdkman.io/usage) to have different Java versions and [Nvm](https://github.com/nvm-sh/nvm) for different Node versions.

```shell
brew leaves > list.txt
cat list.txt
xargs brew desc < list.txt
sdk install java 11.0.11.hs-adpt
```

Brew leaves output:

```shell
awscli
clojure
git
go
gradle
groovy
htop
httpie
hub
jmeter
jq
kafka
kotlin
leiningen
maven
terraform
tree
vert.x
wget
uv
claude-code
copilot-cli
kiro-cli
```
brew install --cask kiro-cli

https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH
https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md

Regarding git, don't forget to set up your username, mail, and editor 

```shell
git config --global core.editor "nano"
```

Finally, you can install all the software that you wanted, and maybe ignore some formulas.

```shell
xargs brew install < list.txt | grep -v ".*full"
```

### AI-Powered Development Tools

Modern development includes AI assistants that boost productivity:

- **Claude Code**: Official Anthropic CLI for AI-assisted development
- **GitHub Copilot CLI**: AI pair programmer in your terminal
- **Kiro CLI**: Modern AWS CLI with better UX

### Model Context Protocol (MCP) Servers

MCPs provide specialized context to AI assistants via `uvx`. Useful servers to configure:

- **AWS Documentation**: AWS service docs and best practices
- **Bedrock AgentCore**: AgentCore platform documentation and APIs
- **GitHub**: Connect Claude Code to repositories


A great example of good onboarding should have to be documented [Onboarding Gitlab](https://about.gitlab.com/handbook/people-group/general-onboarding/)
