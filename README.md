**PRE-REQUISITES:** 
1. Basic knowledge of Knime nodes and operations
2. Knime version later than 4.5.0 (https://www.knime.com/downloads)
3. Python 3 environment with RDKit (version 2021.09.4) and MolVS (version 0.1.1) installed linked to Knime
4. Installing python on Knime: https://docs.knime.com/2018-12/python_installation_guide/index.html
5. Installing RDKit on python: https://www.rdkit.org/docs/Install.html
6. ChemAxon License for Knime Nodes (https://docs.chemaxon.com/display/docs/knime-nodes-licensing.md)


**KNIME EXTENSIONS TO INSTALL:**
1. ChemAxon/Infocom JChem Extensions Feature (https://hub.knime.com/infocom/extensions/jp.co.infocom.cheminfo.jchem.feature/latest)
2. ChemAxon/Infocom Marvin Extensions Feature (ChemAxon/Infocom Marvin Extensions Feature)
3. Indigo Knime Integration (https://hub.knime.com/epam-lsop/extensions/com.epam.indigo.knime.feature/latest)
4. Knime Python Integration (https://hub.knime.com/knime/extensions/org.knime.features.python2/latest)
5. Knime Base Chemistry Types and Nodes (https://hub.knime.com/knime/extensions/org.knime.features.chem.types/latest)
6. RDKit Nodes for Knime (https://hub.knime.com/manuelschwarze/extensions/org.rdkit.knime.feature/latest)
Knime will automatically prompt to install the eventual missing extensions upon workflow importing & opening.

The workflow group **canSARchem.knar** should be imported in LOCAL Knime workspace to avoid setting the path for SDF Reader Nodes inside the Salt Strip metanode (3. Salt Strip Lists) that point to mounting point-relative URL.

The workflow group contains two different pipelines:
1. canSARchem - ChemAxon nodes for which a specific license is needed.
2. canSARchem_RdKit - open-source version of the pipeline - All files written by this pipeline will be added in folders with _RDKit as a suffix (i.e. "Results_RDKit").

**GENERAL MODE OF OPERATION OF THE PIPELINE:**

Starting from an input SDF file, the pipeline will process compounds generating automatically a Result folder in the input path containing a total of at least 4 output sdf.gz files: 1. Standard Compounds; 2. Canonical Representatives; 3. Unsalted Canonical Representative; and 4. Abstract Compounds. For each SDF file, SMILES, InChI & nsInChI, InChI Key & nsInChI Key are calculated. 

The SDF Reader node is set to extract all the properties, but the setting can be easily changed in the dedicated tab. Also, the properties to export are customizable in the SDF writer nodes in the Metanodes.  

Molecules that can't be parsed or can't be read by RDKit or have an empty mol block will be directed to the Errors_No-Structures Metanode. Errors at this stage are indicated by a yellow triangle in the executed Knime nodes. This Metanode creates two additional folders, namely "Errors" and "No_Structures" containing the errors and no-structures output files, respectively. 
Molecules that fail to be standardized will be exported in the Errors folder.

If more than one sdf file is listed in the input directory they will be handled separately. A different output file will be generated for each different input file.
If the input file contains more than 3,000 molecules, it will be split. You will find many output files equal to the times that the file is split (for 10k compounds in the input file you will have 4 output files) to prevent calculation to get stacked. The settings can be customized by choosing the numbers of chunks in the Chunk loop Node inside each of the Output Metanodes.

**EXAMPLE FILES TO TEST THE PIPELINE:**

The 3 sample files furnished in the workflow group contain only one property (chembl_id) that will be used as Molecule name in the output as set in the SDF Writer nodes.
Executing the File Reader Node, if the Workflow group has been imported in the LOCAL Knime Workspace, the example files will be read and can be processed by executing the Variable Condition Loop End Node. Output files will then be generated in the Knime_workspace folder (Knime_Workspace/canSAR_Chemistry_Registration_Pipeline) and will be accessible also directly through Knime LOCAL Workspace from Knime GUI, right-clicking on the canSAR folder and pressing "Refresh" if the created folders do not show. The input path can be changed as desired.
The estimated execution time for the 3 sample files in a workstation with at least 3 available CPUs is 10 minutes.

**STEPS TO RUN THE PIPELINE WITH CUSTOM FILES:**
1. Insert the path of the input file/files in the File Reader node. The pipeline is optimized for recognizing and listing only sdf.gz files, but the extension can be easily modified to sdf in the File Reader Node. Note that the same extension of the input file will be assigned to the output files.

2. Executing the Loop End node, the pipeline will start executing and writing the output files accordingly.  