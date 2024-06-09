to deploy from azure : (web app) Deployment center : 
Settings : 
  Github : change provider => app service build


from :
https://hackernoon.com/resolving-deployment-issues-with-ts-node-and-azure-development-pipelines
<h1>Resolving Deployment Issues with Ts-node and Azure Development Pipelines<br>
by Nuralem Abizov</h1>
<p>In this article, we'll explore common pitfalls and potential solutions when working with <a target="_blank" rel="noopener noreferrer ugc">TypeScript</a> (using ts-node) in <a target="_blank" rel="noopener noreferrer ugc">Azure Serverless</a> Development Pipelines. This piece is particularly valuable for developers encountering problems during the deployment process, often characterized by obscure error messages and puzzling behavior. We'll take a practical, step-by-step approach, investigating these issues, delving into their root causes, and outlining strategies for fixing them.</p>
<p class="line-space"><br>&nbsp;</p>
<p>Whether you're a seasoned developer, DevOps engineer, or a curious beginner eager to learn more about TypeScript and Azure DevOps, this comprehensive guide will serve as a valuable tool in navigating the intricacies of ts-node deployments in Azure pipelines.</p>
<p class="line-space"><br>&nbsp;</p>
<h2>Problem</h2>
<p>Working on <strong>Restart</strong> and building our backend system on different cloud solutions (<a target="_blank" rel="noopener noreferrer ugc">GCP</a>, Azure) our team has faced several issues on serverless deployment.</p>
<p class="line-space"><br>&nbsp;</p>
<p>Using <strong>ts-node</strong> as a runtime in-memory <code>ts-js compiler</code>with the latest TypeScript 5.0.4 and nodejs18, we got errors after successfully building our app using dev.azure.com pipelines (Image 1).</p>
<p><br>&nbsp;</p>
<div class="sc-32effb79-1 pATxT image-container undefined">&nbsp;</div>
<p>&nbsp;</p>
<figure class="image image_resized" style="height:0px;width:0px;"><img class="image" style="aspect-ratio:1080/475;border-width:medium;box-sizing:border-box;display:block;inset:0px;margin:auto;max-height:100%;max-width:100%;min-height:100%;min-width:100%;object-fit:contain;padding:0px;position:absolute;" src="https://hackernoon.imgix.net/images/rD3ZPSbOTDVcZgugSUvaEY2oV0x1-4b93pl2.png?w=1080&amp;q=75&amp;auto=format" alt="Image 1. Successfully deployed app." srcset="https://hackernoon.imgix.net/images/rD3ZPSbOTDVcZgugSUvaEY2oV0x1-4b93pl2.png?w=640&amp;q=75&amp;auto=format 1x, https://hackernoon.imgix.net/images/rD3ZPSbOTDVcZgugSUvaEY2oV0x1-4b93pl2.png?w=1080&amp;q=75&amp;auto=format 2x" sizes="100vw" width="1080" height="475" decoding="async" data-nimg="intrinsic"></figure>
<p>&nbsp;</p>
<div class="sc-32effb79-1 pATxT image-container undefined">
    <p class="image-caption">Image 1. Successfully deployed app.</p>
</div>
<p>&nbsp;</p>
<p><strong>The errors during app startup:</strong></p>
<pre class="language-typescript" tabindex="0"><code class="language-typescript">2023-05-25T10:52:24.326355260Z    _____
2023-05-25T10:52:24.326400861Z   /  _  \ __________ _________   ____
2023-05-25T10:52:24.326406761Z  /  /_\  \\___   /  |  \_  __ \_/ __ \
2023-05-25T10:52:24.326410361Z /    |    \/    /|  |  /|  | \/\  ___/
2023-05-25T10:52:24.326413761Z \____|__  /_____ \____/ |__|    \___  &gt;
2023-05-25T10:52:24.326417661Z         \/      \/                  \/
2023-05-25T10:52:24.326420961Z A P P   S E R V I C E   O N   L I N U X
2023-05-25T10:52:24.326424361Z
2023-05-25T10:52:24.326427461Z Documentation: http://aka.ms/webapp-linux
2023-05-25T10:52:24.326430661Z NodeJS quickstart: https://aka.ms/node-qs
2023-05-25T10:52:24.326433761Z NodeJS Version : v18.16.0
2023-05-25T10:52:24.326436961Z Note: Any data outside '/home' is not persisted
2023-05-25T10:52:24.326440161Z
2023-05-25T10:52:26.504451955Z Starting OpenBSD Secure Shell server: sshd.
2023-05-25T10:52:26.807519399Z Starting periodic command scheduler: cron.
2023-05-25T10:52:26.863716274Z Cound not find build manifest file at '/home/site/wwwroot/oryx-manifest.toml'
2023-05-25T10:52:26.866811884Z Could not find operation ID in manifest. Generating an operation id...
2023-05-25T10:52:26.868247288Z Build Operation ID: 796ee1ba-542e-43f1-9f6c-1e7a5e2e9815
2023-05-25T10:52:27.139574733Z Environment Variables for Application Insight's IPA Codeless Configuration exists..
2023-05-25T10:52:27.156085084Z Writing output script to '/opt/startup/startup.sh'
2023-05-25T10:52:27.229878114Z Running #!/bin/sh
2023-05-25T10:52:27.229938514Z
2023-05-25T10:52:27.229945514Z # Enter the source directory to make sure the script runs where the user expects
2023-05-25T10:52:27.229950214Z cd "/home/site/wwwroot"
2023-05-25T10:52:27.229954114Z
2023-05-25T10:52:27.229957814Z export NODE_PATH=/usr/local/lib/node_modules:$NODE_PATH
2023-05-25T10:52:27.229979514Z if [ -z "$PORT" ]; then
2023-05-25T10:52:27.230057715Z 		export PORT=8080
2023-05-25T10:52:27.230063815Z fi
2023-05-25T10:52:27.230067915Z
2023-05-25T10:52:27.246143265Z PATH="$PATH:/home/site/wwwroot" yarn start
2023-05-25T10:52:30.491250324Z yarn run v1.17.3
2023-05-25T10:52:30.896237571Z $ ts-node src/app.ts
2023-05-25T10:52:31.521499168Z node:internal/modules/cjs/loader:1078
2023-05-25T10:52:31.577313557Z   throw err;
2023-05-25T10:52:31.577384758Z   ^
2023-05-25T10:52:31.577391958Z
2023-05-25T10:52:31.577396558Z Error: Cannot find module './util'
2023-05-25T10:52:31.577400858Z Require stack:
2023-05-25T10:52:31.577404958Z - /home/site/wwwroot/node_modules/.bin/ts-node
2023-05-25T10:52:31.577484858Z     at Module._resolveFilename (node:internal/modules/cjs/loader:1075:15)
2023-05-25T10:52:31.577494058Z     at Module._load (node:internal/modules/cjs/loader:920:27)
2023-05-25T10:52:31.577498458Z     at Module.require (node:internal/modules/cjs/loader:1141:19)
2023-05-25T10:52:31.577502658Z     at require (node:internal/modules/cjs/helpers:110:18)
2023-05-25T10:52:31.577506858Z     at Object.&lt;anonymous&gt; (/home/site/wwwroot/node_modules/.bin/ts-node:9:16)
2023-05-25T10:52:31.577511558Z     at Module._compile (node:internal/modules/cjs/loader:1254:14)
2023-05-25T10:52:31.577551858Z     at Module._extensions..js (node:internal/modules/cjs/loader:1308:10)
2023-05-25T10:52:31.577557158Z     at Module.load (node:internal/modules/cjs/loader:1117:32)
2023-05-25T10:52:31.577561358Z     at Module._load (node:internal/modules/cjs/loader:958:12)
2023-05-25T10:52:31.577565458Z     at Function.executeUserEntryPoint [as runMain] (node:internal/modules/run_main:81:12) {
2023-05-25T10:52:31.577569658Z   code: 'MODULE_NOT_FOUND',
2023-05-25T10:52:31.577573658Z   requireStack: [ '/home/site/wwwroot/node_modules/.bin/ts-node' ]
2023-05-25T10:52:31.577577758Z }
2023-05-25T10:52:31.577586558Z
2023-05-25T10:52:31.577591058Z Node.js v18.16.0
2023-05-25T10:52:31.684203528Z error Command failed with exit code 1.
</code></pre>
<p class="line-space"><br>&nbsp;</p>
<p><strong>The key points of these logs are:</strong></p>
<pre class="language-typescript" tabindex="0"><code class="language-typescript">Error: Cannot find module './util'
</code></pre>
<pre class="language-typescript" tabindex="0"><code class="language-typescript">/home/site/wwwroot/node_modules/.bin/ts-node
</code></pre>
<pre class="language-typescript" tabindex="0"><code class="language-typescript">requireStack: [ '/home/site/wwwroot/node_modules/.bin/ts-node' ]
</code></pre>
<pre class="language-typescript" tabindex="0"><code class="language-typescript">error Command failed with exit code 1.
</code></pre>
<p class="line-space"><br>&nbsp;</p>
<p class="line-space"><br>&nbsp;</p>
<p>Spending hours of googling and searching for similar problems on Stack Overflow, we cannot</p>
<p>define the nature of the issue. Trying different pipeline setups, and switching from Linux based web app to Windows does not help. The same code works fine in the case of serverless Google Cloud, but for some reason does not work on serverless Azure.</p>
<p class="line-space"><br>&nbsp;</p>
<p>Finally, we got a solution: We switch <code>“ts-node”</code> to a basic <code>“tsc“</code> compiler and everything worked fine.</p>
<p class="line-space"><br>&nbsp;</p>
<p>In this article, I will explain some details of switching your existing project, that uses <code>ts-node</code> to a native <code>tsc</code> compiler.</p>
<p class="line-space"><br>&nbsp;</p>
<h2>Tiny ts-node project</h2>
<p>First, we need a basic project that has only one file and ts-node on it (Image 2).</p>
<p class="line-space"><br>&nbsp;</p>
<p>&nbsp;</p>
<div class="sc-32effb79-1 pATxT image-container undefined">&nbsp;</div>
<p>&nbsp;</p>
<figure class="image image_resized" style="height:0px;width:0px;"><img class="image" style="aspect-ratio:828/556;border-width:medium;box-sizing:border-box;display:block;inset:0px;margin:auto;max-height:100%;max-width:100%;min-height:100%;min-width:100%;object-fit:contain;padding:0px;position:absolute;" src="https://hackernoon.imgix.net/images/rD3ZPSbOTDVcZgugSUvaEY2oV0x1-xta3pni.png?w=828&amp;q=75&amp;auto=format" alt="Image 2. Project structure." srcset="https://hackernoon.imgix.net/images/rD3ZPSbOTDVcZgugSUvaEY2oV0x1-xta3pni.png?w=640&amp;q=75&amp;auto=format 1x, https://hackernoon.imgix.net/images/rD3ZPSbOTDVcZgugSUvaEY2oV0x1-xta3pni.png?w=828&amp;q=75&amp;auto=format 2x" sizes="100vw" width="828" height="556" decoding="async" data-nimg="intrinsic"></figure>
<p>&nbsp;</p>
<div class="sc-32effb79-1 pATxT image-container undefined">
    <p class="image-caption">Image 2. Project structure.</p>
</div>
<p>&nbsp;</p>
<p class="line-space"><br>&nbsp;</p>
<p class="line-space"><br>&nbsp;</p>
<p>The <code>src</code> folder contains our app.ts file:</p>
<pre class="language-typescript" tabindex="0"><code class="language-typescript">import * as http from 'http';

const server = http.createServer((req, res) =&gt; {
    res.statusCode = 200;
    res.setHeader('Content-Type', 'text/plain');
    res.end('Hello Hackernoon. We are using ts on Azure!\n');
});

server.listen(8080, '0.0.0.0', () =&gt; {
    console.log('Server running at http://0.0.0.0:8080/');
});
</code></pre>
<p class="line-space"><br>&nbsp;</p>
<p>In <code>.gitignore</code> we have added <code>dist</code> folder and <code>node_modules</code> because compiling from <code>ts</code> to <code>js</code> will be inside the Azure pipeline process.</p>
<p class="line-space"><br>&nbsp;</p>
<p><strong>The content of package.json:</strong></p>
<pre class="language-typescript" tabindex="0"><code class="language-typescript">{
  "name": "ts-nodejs",
  "version": "1.0.0",
  "description": "",
  "scripts": {
    "start": "ts-node src/app.ts"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "ts-node": "^10.9.1",
    "typescript": "^5.0.4"
  },
  "devDependencies": {
    "@types/node": "^20.2.3"
  }
}
</code></pre>
<p class="line-space"><br>&nbsp;</p>
<p>We have installed only ts-node and typescript packages.</p>
<p class="line-space"><br>&nbsp;</p>
<p><strong>First, we need to modify the</strong> <code>start</code> <strong>command and create a new command and add it here:</strong></p>
<pre class="language-typescript" tabindex="0"><code class="language-typescript">{
  "name": "tsc-nodejs",
  "version": "1.0.0",
  "description": "",
  "scripts": {
    "start": "node dist/app.js",
    "build": "tsc"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "ts-node": "^10.9.1",
    "typescript": "^5.0.4"
  },
  "devDependencies": {
    "@types/node": "^20.2.3"
  }
}
</code></pre>
<p class="line-space"><br>&nbsp;</p>
<p>All project pre-compiled files will be stored in the <code>dist</code> folder, which <code>nodejs</code> will execute our entry point of the app (app.js). <code>Build</code> the command executes or <code>tsc</code> compiler. It gets settings from <code>tsconfig.json</code> file.</p>
<p class="line-space"><br>&nbsp;</p>
<p><strong>Our settings will be:</strong></p>
<pre class="language-typescript" tabindex="0"><code class="language-typescript">{
  "compilerOptions": {
    "target": "ES2019",
    "module": "commonjs",
    "esModuleInterop": true,
    "allowUnreachableCode": false,
    "allowUnusedLabels": false,
    "noFallthroughCasesInSwitch": true,
    "noImplicitAny": false,
    "noImplicitReturns": true,
    "noImplicitThis": true,
    "noUnusedLocals": true,
    "strictBindCallApply": true,
    "strictFunctionTypes": true,
    "strictNullChecks": true,
    "strictPropertyInitialization": true,
    "removeComments": true,
    "strict": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "outDir": "dist",
    "baseUrl": "./src",
    "rootDir": "src",
    "paths": {
      "@extensions/*": ["handlers/extensions/*"],
      "@services/*": ["services/*"],
      "@docs/*": ["docs/*"],
      "@server/*": ["server/*"],
      "@handlers/*": ["handlers/*"],
      "@app/*": ["*"],
    },
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true,
    "typeRoots": [
      "./types",
      "./node_modules/@types"
    ],
    "allowSyntheticDefaultImports": true,
    "lib": ["es5", "es6", "dom"],
    "moduleResolution": "node",
  },
  "include": [
    "**/*"
  ],
  "exclude": [
    "dist",
    "node_modules"
  ]
}
</code></pre>
<p class="line-space"><br>&nbsp;</p>
<p><strong>The most important parts of the tsconfig.json:</strong></p>
<pre class="language-typescript" tabindex="0"><code class="language-typescript">;"noEmit": false,
 "include": [
    "**/*"
  ],
"outDir": "dist",
"baseUrl": "./src",
"rootDir": "src",
</code></pre>
<p class="line-space"><br>&nbsp;</p>
<p>Be sure, that <code>noEmit</code> is false (by default is false), <code>outDir</code> is the name of the folder, where <code>tsc</code> will save precompiled files, <code>baseUrl</code> and <code>rootDir</code> is <code>src</code>, where your project is (don’t save project files outside <code>src</code> folder, because <code>tsc</code> will ignore them).</p>
<p class="line-space"><br>&nbsp;</p>
<p>In this example, I also have added<code>paths</code>in case you have it on an existing project, but this tiny project does not use its allies, so you can ignore them. If you use allies on your project, please change them inside <code>package.json</code> from:</p>
<pre class="language-typescript" tabindex="0"><code class="language-typescript">"_moduleAliases": {
    "@extensions": "src/handlers/extensions",
    "@services": "src/services",
    "@docs": "src/docs",
    "@server": "src/server",
    "@handlers": "src/handlers",
    "@app": "src"
  }
</code></pre>
<p class="line-space"><br>&nbsp;</p>
<p><strong>to this:</strong></p>
<pre class="language-typescript" tabindex="0"><code class="language-typescript">"_moduleAliases": {
    "@extensions": "dist/handlers/extensions",
    "@services": "dist/services",
    "@docs": "dist/docs",
    "@server": "dist/server",
    "@handlers": "dist/handlers",
    "@app": "dist"
  }
</code></pre>
<p class="line-space"><br>&nbsp;</p>
<p class="line-space"><br>&nbsp;</p>
<p>Finally, you need to change basic <code>azure-pipelines.yaml</code> config from:</p>
<pre class="language-typescript" tabindex="0"><code class="language-typescript"># Node.js Express Web App to Linux on Azure
# Build a Node.js Express app and deploy it to Azure as a Linux web app.
# Add steps that analyze code, save build artifacts, deploy, and more:
# https://docs.microsoft.com/azure/devops/pipelines/languages/javascript

trigger:
- master

variables:

  # Azure Resource Manager connection created during pipeline creation
  azureSubscription: '&lt;Your sub&gt;'

  # Web app name
  webAppName: '&lt;Your webAppName&gt;'

  # Environment name
  environmentName: 'Your webAppName&gt;'

  # Agent VM image name
  vmImageName: 'ubuntu-latest'

stages:
- stage: Build
  displayName: Build stage
  jobs:
  - job: Build
    displayName: Build
    pool:
      vmImage: $(vmImageName)

    steps:
    - task: NodeTool@0
      inputs:
        versionSpec: '18.x'
      displayName: 'Install Node.js'

    - script: |
        npm install yarn
        yarn
      displayName: 'npm install, build and test'

    - task: ArchiveFiles@2
      displayName: 'Archive files'
      inputs:
        rootFolderOrFile: '$(System.DefaultWorkingDirectory)'
        includeRootFolder: false
        archiveType: zip
        archiveFile: $(Build.ArtifactStagingDirectory)/$(Build.BuildId).zip
        replaceExistingArchive: true

    - upload: $(Build.ArtifactStagingDirectory)/$(Build.BuildId).zip
      artifact: drop

- stage: Deploy
  displayName: Deploy stage
  dependsOn: Build
  condition: succeeded()
  jobs:
  - deployment: Deploy
    displayName: Deploy
    environment: $(environmentName)
    pool:
      vmImage: $(vmImageName)
    strategy:
      runOnce:
        deploy:
          steps:
          - task: AzureWebApp@1
            displayName: 'Azure Web App Deploy: Your webAppName&gt;'
            inputs:
              azureSubscription: $(azureSubscription)
              appType: webAppLinux
              appName: $(webAppName)
              runtimeStack: 'NODE|18-lts'
              package: $(Pipeline.Workspace)/drop/$(Build.BuildId).zip
              startUpCommand: 'yarn start'
</code></pre>
<p class="line-space"><br>&nbsp;</p>
<p><strong>to this:</strong></p>
<pre class="language-typescript" tabindex="0"><code class="language-typescript"># Node.js Express Web App to Linux on Azure
# Build a Node.js Express app and deploy it to Azure as a Linux web app.
# Add steps that analyze code, save build artifacts, deploy, and more:
# https://docs.microsoft.com/azure/devops/pipelines/languages/javascript

trigger:
- master

variables:

  # Azure Resource Manager connection created during pipeline creation
  azureSubscription: 'Your sub'

  # Web app name
  webAppName: 'Your webAppName&gt;'

  # Environment name
  environmentName: 'Your webAppName&gt;'

  # Agent VM image name
  vmImageName: 'ubuntu-latest'

stages:
- stage: Build
  displayName: Build stage
  jobs:
  - job: Build
    displayName: Build
    pool:
      vmImage: $(vmImageName)

    steps:
    - task: NodeTool@0
      inputs:
        versionSpec: '18.x'
      displayName: 'Install Node.js'

    - script: |
        npm install yarn
        yarn
        yarn build
      displayName: 'npm install, build and test'

    - task: ArchiveFiles@2
      displayName: 'Archive files'
      inputs:
        rootFolderOrFile: '$(System.DefaultWorkingDirectory)'
        includeRootFolder: false
        archiveType: zip
        archiveFile: $(Build.ArtifactStagingDirectory)/$(Build.BuildId).zip
        replaceExistingArchive: true

    - upload: $(Build.ArtifactStagingDirectory)/$(Build.BuildId).zip
      artifact: drop

- stage: Deploy
  displayName: Deploy stage
  dependsOn: Build
  condition: succeeded()
  jobs:
  - deployment: Deploy
    displayName: Deploy
    environment: $(environmentName)
    pool:
      vmImage: $(vmImageName)
    strategy:
      runOnce:
        deploy:
          steps:
          - task: AzureWebApp@1
            displayName: 'Azure Web App Deploy: Your webAppName&gt;'
            inputs:
              azureSubscription: $(azureSubscription)
              appType: webAppLinux
              appName: $(webAppName)
              runtimeStack: 'NODE|18-lts'
              package: $(Pipeline.Workspace)/drop/$(Build.BuildId).zip
              startUpCommand: 'yarn start'
</code></pre>
<p class="line-space"><br>&nbsp;</p>
<p>Finally, <code>git push</code> and see what will happen!</p>
<h2>Conclusion</h2>
<p>Hope this article will save a lot of time during debugging process in case of using <code>ts-node</code> with <code>Azure Node.js Serverless Apps</code>. It's clear that the complexities of TypeScript deployments within Azure DevOps pipelines can present unique challenges. However, by taking a systematic approach and learning to understand the underpinnings of ts-node and Azure pipelines, we can effectively troubleshoot and resolve these obstacles.</p>
<p class="line-space"><br>&nbsp;</p>
<p>In this article, we dove into the most common issues developers encounter, broke down the intricacies of these problems, and provided step-by-step solutions. Remember, it's not about avoiding problems altogether – it's about developing the ability to diagnose, troubleshoot, and solve them effectively when they inevitably arise.</p>
<p class="line-space"><br>&nbsp;</p>
<p>Moving forward, take this knowledge and apply it to your DevOps processes. Not only will you increase your productivity, but you'll also be contributing to a smoother, more efficient pipeline for your entire team. There's always more to learn in this ever-evolving field, and this guide is just one step towards mastering Azure DevOps with TypeScript.</p>
<p>Stay curious, keep learning, and happy coding!</p>
