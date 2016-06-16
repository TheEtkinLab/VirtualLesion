# Virtual Lesion
This series of notebooks is used to determine the evidence for a pathway connecting two ROIs following those described in Pestilli et al.<sup>[1]</sup> and Leong et al.<sup>[2]</sup>.

Required steps before running these notebooks:

<ol>
    <li>Regions of interest (ROIs) were defined in MNI space and subsequently warped from MNI space to each individual subject’s native space (e.g., fsl5.0-applywarp -i roi.nii.gz  -r T1w/Diffusion/nodif_brain_mask.nii.gz -w MNINonLinear/xfms/standard2acpc_dc.nii.gz -o roi_warped)</li>
    <li>Constrained spherical deconvolution (CSD)<sup>[3]</sup> was used to estimate the fiber orientation distribution function (fODF). The fODFs were combined with tissue segmentations derived from the high resolution T1-weighted images to perform anatomically-constrained tractography<sup>[4]</sup></li>
    <li>A whole-brain connectome was then generated by seeding all voxels in the white matter. These were filtered down using spherical-deconvolution informed filtering of tractograms (SIFT)<sup>[5]</sup> </li>
</ol>

Calculation steps done in these notebooks:

<ol>
    <li>Virtual_Lesion_Step1</li>
    <li>Virtual_Lesion_Step1
        <ol>
            <li>Generate fiber tracks by seeding from ROI1 and selecting only those that pass through ROI2, and vise versa, with MRtrix3</li>
            <li>Remove the fibers from the previous step that do not start in one ROI and end in the other (The fibers that pass through the ROIs but do not terminate in them)</li>
            <li>Combine all valid fiber tracks into one candiadate streamline set</li>
            <li>Cluster the candiadate streamlines with the dipy<sup>[6]</sup> QuickBundles algorithm<sup>[7]</sup> to remove outliers</li>
        </ol>
    </li>
    <li>Virtual_Lesion_Step2</li>
    <li>Virtual_Lesion_Step2
        <ol>
            <li>Calculate the neighborhood of the candidate streamlines (Voxels where the streamlines pass through)</li>
            <li>Find fibers in the whole brain connectome that pass through the neibghorhood</li>
            <li>Combine whole brain connectome with candidate streamlines into one steamline set</li>
            <li>Run the LiFE optimization on this combined streamline set and retrieve RMSE between the original data set and the LiFE prediction</li>
            <li>Run the LiFE otimization and RMSE calculation again on the whole brain, but this time without the candidate streamlines</li>
        </ol>
    </li>
    <li>Virtual_Lesion_Step3</li>
    <li>Virtual_Lesion_Step3
        <ol>
            <li>Calculating the normalized difference of the two RMSEs provides the Strength of Evidence</li>
            <li>Save the LiFE optimized candidate streamlines between the two ROIs</li>
        </ol>
    </li>
</ol>

**<small>Do not run the parallization on all cores. This notebook is parallalized by subjects. The limiting factor is the memory usage per process.</small>

<sup>[1]</sup> <i>PMID: 25194848</i> <br/>
<sup>[2]</sup> <i>PMID: 26748088</i> <br/>
<sup>[3]</sup> <i>PMID: 18583153</i> <br/>
<sup>[4]</sup> <i>PMID: 22705374</i> <br/>
<sup>[5]</sup> <i>PMID: 23238430</i> <br/>
<sup>[6]</sup> <i>PMID: 24600385</i> <br/>
<sup>[7]</sup> <i>PMID: 23248578</i> <br/>
