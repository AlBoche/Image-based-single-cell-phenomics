This interactive pipeline is a semi-manual macro on FIJI [1] to extract different features from FIJI to a phenomics analysis.
In order to use it, the user must prepare a file accordingly:

In the order "Experiment => Cell Line => Condition", user have to prepare three other files "Cell", "Nucleus" and "Fresque". 
Additionnaly, user will also place three TIF files: the microscopy image, along with the cellular and nuclear segmentation made on Napari with cellpose [2].

<img width="536" alt="image" src="https://github.com/AlBoche/Image-based-single-cell-phenomics/assets/165380486/acb53042-f624-4537-a30c-b9e271389440">

After opening the image on FIJI, user can launch the macro and indicates the folder location, except for the condition part.

<img width="377" alt="image" src="https://github.com/AlBoche/Image-based-single-cell-phenomics/assets/165380486/366844de-bb14-48c2-afa4-a26be0c44ec1">

Followed by instructions on the image: what are the channels, how precise you expect the focal points to be detected (through local maxima analysis), or what are the name of the cell line, condition and replicate number of your experiment.

<img width="302" alt="image" src="https://github.com/AlBoche/Image-based-single-cell-phenomics/assets/165380486/64e1bf2b-a4de-49a0-805b-2884be1820de">

After completing this second part, the macro will start by combining the two sets of segmentation into a single ROI Manager (ROI=Region Of Interest), with the nucleus ordered exactly like the segmented cells. Doing so can create troubles since some nucleus contours might overlap on other cell, or even single pixel created on Napari perturbing the algorithm. To deal with such trouble, the macro will look for a correction by reducing then enlarging the cells contours generated to delete the abnormal pixel sized nucleus. At the end of the first detection, a second part will be made and a manual prompt will appear. The list of the remaining "anomalities" will be prompted too on the log, and user can look specifically to these nucleus instead of a long "eye-checking" screening. 

<img width="892" alt="image" src="https://github.com/AlBoche/Image-based-single-cell-phenomics/assets/165380486/4ae2111f-0b07-4e68-98a5-84ef17e4e677">

Once it is done (either on FIJI for nucleus, or by restarting a napari segmentation to correct cell shape more precisely than what can be done on FIJI), a last request will be made to tell the macro if modifications has been made. If none, simply press "OK". If modifications have been made:
- for nucleus: simply check the first box.
- for cells: please enter the number of cells you have discarded or added

- <img width="277" alt="Capture d'écran 2024-04-02 114942" src="https://github.com/AlBoche/Image-based-single-cell-phenomics/assets/165380486/1bb2dbb2-e560-4379-ab26-f88bf1fba2e6">

After checking, an additionnal correction will be made if modifications have been made.
Will start the regular FIJI measurement for the channels registered at the beginning of the process, followed by the extration of the binarized contours for nucleus and cell in order to use Celltool [3] for a morphological PCA (not included directly on this pipeline). A png save of each cell for an "analysis-ending" stage is also made, in order to visualize cell directly on the UMAP single cell clustering obtained at the end of the analysis. (See example below).

https://github.com/AlBoche/Image-based-single-cell-phenomics/assets/165380486/19b1efdf-4dc7-40b3-a0de-e0e24a24089d

Finally for the semi manual part, a last box will be prompted to ask for the intra-normalization process. By default, nucleus are interpolated for each pixel, while cells only one in ten pixels. I recommand ten as it is sufficiant. You can reduce the cellular interpolation for a smoother result but the cost in time will also increase. 

<img width="204" alt="Capture d'écran 2024-04-02 120312" src="https://github.com/AlBoche/Image-based-single-cell-phenomics/assets/165380486/a4ac9324-f62d-40c6-aa44-b20c1dc8cbc0">

Finally the fragmentation will normalize the distance from nucleus to cell membrane in the whole cell. If checked, the local points will also be measure within the same normalized range (used for dot-like information, like ZO-1 for tight junction in our example). At the end of the analysis, you obtain the three folders containing all the binary contours and the png of cells. Along with it, you get the PNG version of the whole image (used during the single-cell PNG acquisition) and the TIF image with all the nucleus/cell fragmented in case you have to correct anything to not relaunch the whole analysis. You also obtain the regular measure of FIJI for each cell and nucleus, with nucleus in the same order than cell, so you can combine them easily in the csv file. Furthermore, you obtain the information for each ROI within each cell for the wanted channels (second request to user in the macro) and the local maxima as well if cheched.

<img width="755" alt="image" src="https://github.com/AlBoche/Image-based-single-cell-phenomics/assets/165380486/c9c00a06-bb9f-44f3-85b4-84536af59bca">

Hope this will help you for your analysis !

[1]Schindelin, J., Arganda-Carreras, I., Frise, E., Kaynig, V., Longair, M., Pietzsch, T., Preibisch, S., Rueden, C., Saalfeld, S., Schmid, B., et al. (2012). Fiji: an open-source platform for biological-image analysis. Nat. Methods 9, 676–682. 10.1038/nmeth.2019

[2]Pachitariu, M., and Stringer, C. (2022). Cellpose 2.0: how to train your own model. Nat. Methods 19, 1634–1641. 10.1038/s41592-022-01663-4

[3]Pincus, Z., and Theriot, J.A. (2007). Comparison of quantitative methods for cell-shape analysis. J. Microsc. 227, 140–156. 10.1111/j.1365-2818.2007.01799.x
