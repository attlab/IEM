##### Purpose: a general and flexible function for doing inverted encoding models.
##### Needs: a flexible function that can work with 1 or more conditions, variable numbers of electrodes, different EEG signals. Assuming 1 subject at a time.
##### Outputs: Channel response function structure, with fields

1) First list item   
   - First nested list item 
      - Second nested list item

1) Setting paths and dependencies   
   a) Input: actual pathnames and dependencies   
   b) Output: EM structure (encoding model)
     i) Datapath, analysispath, sjID, dependencies  
2)	Set analysis parameters
	a) Input: EM structure from step 1
	b) Use IEM_settings (Tom’s, includes basis set, freqs of interest, etc.)
	c) Number of conditions included, fixed or independent variable
	d) Cross-validation type
	e) Output: Exploded fieldname variables from the EM structure (this is convenience so one doesn’t have to refer to the structure.fieldname everytime).
3) Get the data
	a) Input: paths and preprocessing info from exploded EM struct
	b) Load the data file
	c) Assuming preprocessed data stored in a particular format
	d) Deal with last stages of preprocessing (e.g., remove rejected trials, deal with bad channels, etc.)
	e) Number of trials for each condition and each bin of the stimulus space (include check for whether the ntrials is reasonable)
	f) If needed, rearrange dimensions of the data matrix to be ntrials x nelecs x ntimepoints
	g) Output: ntrials x nelecs x ntimepoints data matrix, and stuff from (e). eeg
3) Loop through frequencies and n basis sets
	a) Put the data into frequency domain
		i) Input: eeg from step 3, relevant info from EM structure
		ii) Butterworth
		iii) Hilbert
		iv) “Downsample” if desired
		v) Output: eegFreqDecomp ntrials x nelecs x ntimes
	b) Do the modelling
		i) Input: eegFreqDecomp, basis set
		ii) If doing block method (e.g., Foster et al., 2016), do it and iterate
		iii) Structure the data for the cross-validation type
		iv) Convert to power (total and/or evoked)
		v) Loop through cross-vals (i.e., LOO or k-fold)
		vi) Do IEM
			Select training data
			Select test data
			Estimate weights
			Invert on test data
			To shift or not to shift
		vii) Output: centered and/or uncentered CRFs
5) Cleanup
	a) Package all the analysis details & results into the EM structure
	b) Save
	c) Ciao

