## Defacing and De-Identification

*Please note that de-identification policies can vary between studies and institutions. Be sure to check the specific IRB requirements for your study to ensure your data is thoroughly de-identified according to those standards.*

### Defacing

Defacing is performed in order to mask out the eyes, nose, and mouth from MRI images. These features could hypothetically be used to reconstruct someone's facial structure, which is considered indirect identifiable information. Skull-stripping can also be used for defacing, however this limits what kind of preprocessing can be performed with the data in the event that head images are required.

There are several tools available for defacing, but we recommend using [Pydeface](https://pypi.org/project/pydeface/) (also see the [Neuroimaging Cookbook](https://neuroimaging-cookbook.github.io/recipes/pydeface_recipe/) for more thorough instruction). Pydeface registers a default atlas to the subject head and applies the resulting linear transform to the atlas' accompanying defaced mask to create a subject-specific mask. Pydeface takes one input, the subject head file (see usage for additional options), so simply `pip install pydeface` and run:

```
pydeface subject_head.nii.gz
```

**Specific instructions for infant data**

Defacing infant data is more difficult compared to adults due to contrast inversion (and variable or lower contrast depending on the age and modality) in T1w and T2w images over the first 6 months of age, so it's best to use age-specific atlases in order to get good results. We have generated defaced masks for the 0-2 month old infant NIH MNI T1w and T2w atlases here: `/home/faird/shared/code/internal/utilities/deface/infant_templates` (details for how the defaced masks were generated are included in the README in this directory).    

```
pydeface <T1w subject head> --template <INFANT_MNI_T1_1mm.nii.gz> --facemask <INFANT_MNI_T1_1mm_defaced_mask.nii.gz>
```

Note that results may vary in infants even when using age-specific atlases. If your T1w and T2w files are registered, one method you can try is to generate a defaced mask for whichever modality resulted in better masking (i.e. more thorough masking of the eyes, nose, and mouth without cutting off any regions of the brain) and apply that mask to the less successful image modality.  

### Removing identifying information from filenames and metadata
In addition to defacing, you may also need to strip PHI from MRI filenames and metadata. For DICOM anonymization, we’ve found [DICAT](https://github.com/aces/DICAT) to be effective and well-documented.
If you have other recommendations or helpful information you would like to add to this section, please post an issue to the [CDNI Brain GitHub](https://github.com/DCAN-Labs/cdni-brain/issues)!   
