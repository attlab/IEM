##### Purpose: a general and flexible function for doing inverted encoding models.
##### Needs: a flexible function that can work with 1 or more conditions, variable numbers of electrodes, different EEG signals. Assuming 1 subject at a time.
##### Outputs: Channel response function structure, with fields

1.	Setting paths and dependencies   
   	- Input: actual pathnames and dependencies   
   	- Output: EM structure (encoding model)
    	- Datapath, analysispath, sjID, dependencies  
2.  Set analysis parameters
	- Input: EM structure from step 1
	-  Use IEM_settings (Tom’s, includes basis set, freqs of interest, etc.)
	- Number of conditions included, fixed or independent variable
	- Cross-validation type
	- Output: Exploded fieldname variables from the EM structure (this is convenience so one doesn’t have to refer to the structure.fieldname everytime).
3.  Get the data
	- Input: paths and preprocessing info from exploded EM struct
	- Load the data file
	- Assuming preprocessed data stored in a particular format
	- Deal with last stages of preprocessing (e.g., remove rejected trials, deal with bad channels, etc.)
	- Number of trials for each condition and each bin of the stimulus space (include check for whether the ntrials is reasonable)
	- If needed, rearrange dimensions of the data matrix to be ntrials x nelecs x ntimepoints
	- Output: ntrials x nelecs x ntimepoints data matrix, and stuff from (e). eeg
4.  Loop through frequencies and n basis sets
	-   Put the data into frequency domain
	    - Input: eeg from step 3, relevant info from EM structure
		- Butterworth
		- Hilbert
		- “Downsample” if desired
		- Output: eegFreqDecomp ntrials x nelecs x ntimes
	-   Do the modelling
		- Input: eegFreqDecomp, basis set
		- If doing block method (e.g., Foster et al., 2016), do it and iterate
		- Structure the data for the cross-validation type
		- Convert to power (total and/or evoked)
		- Loop through cross-vals (i.e., LOO or k-fold)
		- Do IEM
		  - Select training data
		  - Select test data
		  -	Estimate weights
		  -	Invert on test data
		  -	To shift or not to shift
		- Output: centered and/or uncentered CRFs
5. Cleanup
   - Package all the analysis details & results into the EM structure
   - Save
   - Ciao

