## OptiFine Decompilation Process

### Prerequisites:

1. Make sure that the Gradle "setup" task in MCP-reborn is successful.
2. Minecraft launches without any issues.

### Tools and Files
#### Tools:
1. [MCP-Reborn (Minecraft Coder Pack - Reborn)](https://github.com/Hexeption/MCP-Reborn)
2. ForgeAutoRenamingTool-1.0.2-all.jar
3. ForgeFlower-2.0.629.0.jar
4. [Tools](https://github.com/0deans/Tools)
#### Files:
1. [OptiFine](https://www.optifine.net/downloads)
2. client.jar: Located in <path_to_your_MCP-Reborn>\projects\mcp\build\mcp\downloadClient
3. merge.jar: Located in <path_to_your_MCP-Reborn>\projects\mcp\build\mcp\merge
4. mergeMappings.jar.mappings: Located in <path_to_your_MCP-Reborn>\projects\mcp\build\mcp\mergeMappings
5. libraries.txt: Located in <path_to_your_MCP-Reborn>\projects\mcp\build\mcp\listLibraries
6. mcp_config.zip: Located in <path_to_your_MCP-Reborn>\projects\mcp
7. mappings.zip: Located in <path_to_your_MCP-Reborn>\build

### Steps
#### Step 1: Patching with OptiFine

Use OptiFine to apply the necessary patches. The patched Minecraft files will be neatly stored in the "patched.jar/notch" directory.

```bash
java -cp "optifine.jar" optifine.Patcher client.jar optifine.jar patched.jar
```

#### Step 2: Working with files and renaming

1. Copy the files from the "patched.jar/notch" directory to the "merge.jar" file (replace if necessary).
2. Save the "assets" folder from "patched.jar". You will need this folder later.

```bash
java -jar ForgeAutoRenamingTool-1.0.2-all.jar --input merge.jar --output rename.jar --map mergeMappings.jar --cfg libraries.txt --ann-fix --ids-fix --src-fix --record-fix
```

#### Step 3: Decompiling

Decompile the "rename.jar" file. The result will be the "decompiled.jar" file.

```bash
java -jar forgeflower-2.0.629.0.jar -din=1 -rbr=1 -dgs=1 -asc=1 -rsy=1 -iec=1 -jvn=1 -isl=0 -iib=1 -bsm=1 -dcl=1 -log=TRACE -cfg libraries.txt rename.jar decompiled.jar
```

#### Step 4: Patching Again

Apply the patches from "mcp_config.zip/patches/joined" to the decompiled "decompiled.jar" file.
Extract the "joined" folder from the archive before running the command.

```bash
java -cp Tools.jar ApplyPatches decompiled.jar patched_joined joined rejects_joined
```

### Step 5: Add the "mcp" folder from "mcp_config.zip/config/inject" to the "patched_joined" folder

### Step 6: Rename SRG Names

```bash 
java -cp Tools.jar Srg2Mcp mappings.zip patched_joined
```

#### Completion

After completing these steps, you may need to fix some issues with your project.
The final source code is placed in the "patched_joined" folder.

### Additional
#### To generate new patches, use the following command:

```bash
java -cp Tools.jar GeneratePatches patched_joined output.zip src
```

#### Using Previously Generated Patches

If you have previously generated patches between 'patched_joined' and a source with fixed project problems, you can use them as follows:

```bash
java -cp Tools.jar ApplyPatches src patched_output patches rejects
```
