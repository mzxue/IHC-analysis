#####If you have any questions on this IHC analysis system,
	please contact Dr.Xue by xuemz@sari.ac.cn#############

!!!!!!!Only for non-commercial use!!!!!!!

1. All source codes are in the folder named of "analysis", while model data are stored in the remain folders. In details, "compound" is for nucleus and cytoplasm stained IHC images such as ones from BRCC36 datasets, "membrane" is for membrane stained IHC images with representatives of HER2 images, and "nucleus" is for nucleus stained IHC images like ER images.

2. Ensure that you have installed python-2.7, python-imaging-library-1.1.7, python-mahotas-1.0.4, python-opencv-2.4.7, python-sklearn-0.14.1 and dependencies such as numpy, scipy and matplotlib. Specifically for image segmentation, you will need superpixel-1.1 (Veksler,et al., 2010).

3. For analysis for only one image, you can use source codes in the folder "analysis/forone"
	3.1 command: python segmentation.py xx.png parameter.txt
	    result: xx.pgm, xx_segout.ppm

	3.2 command: python getpatches.py parameter.txt xx_segout.ppm xx.png
	    result: patch3030/xx_segout/xx_patchyposition_rgb.txt

	3.3 command: ls fullpath/*_rgb.txt > rgbtxt.list
		     python featurecom.py rgbtxt.list parameter.txt
	    result: patch3030/xx_segout/xx_patchyposition_rgbshiftsizeHSVhistparam2.ppm, xx_patchyposition_rgbshiftsize.ppm, xx_patchypositionrgbshiftsize2DHS-V_values_param2.txt, xx_patchypositionallfilterimg_lbphist.txt, xx_patchyposition_siftkpdes.txt
	
	3.4 command: python combinefea.py rgbtxt.list
	    result: xx_allpatchfeature.txt

	3.5 command: python selcancer.py pcamodelf n_com xx_allpatchfeature.txt cancer_pcaf #for example, ../compound/brcc36_pca.txt 1385 xx_allpatchfeature.txt ../compound/brcc36_cancer_pcaconvt.txt
	    result: xxvar99_decbypca.data, xxvar99_adjcosdis.data, xx_potentialcancer_adjcos.dis

	3.6 command: python IHCscoring.py indexf xx_allpatchfeature.txt xx_potentialcancer_adjcos.dis svmmodelf labelf xx.png rgbtxt.list # for example, ../compound/brcc36_feasel.index, xx_allpatchfeature.txt, xx_potentialcancer_adjcos.dis, ../compound/brcc36_superp_bestdim_forsvm.txt, ../compound/brcc36_5class.label, xx.png, rgbtxt.list
	    result: xx_feaselcancer.txt, xx_cancerpatch_rgb.txt, xx_superp_feasel.txt, xxscorebyLSVC.txt, xx_cancerpatch.png

4. For analysis for two or more images, you can use source codes in the folder "analysis/multiimgs" with the same implementation.
	Difference:
	4.1 python segmentation.py pnglist.txt parameter.txt
	4.2 python getpatches.py parameter.txt pnglist.txt
	4.3 python mkrgbtxt.py pnglist.txt parameter.txt
	    ls *rgblist.txt > rgblist.txt
	4.4 python mainfeature.py rgblist.txt parameter.txt
	4.5 python selcancer.py pnglist.txt pcamodelf n_com cancer_pcaf
	4.6 python IHCscoring.py pnglist.txt rgblist.txt indexf svmmodelf labelf 
