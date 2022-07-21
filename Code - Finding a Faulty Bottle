% Embeded Image Processing - Assignment 1
% Name - Amogh M. Agnihotri
% Student ID - 21236437
% Course - Master of Science - Artificial Intelligence


%To clean everthing before the fresh run
clear;
clc;
close all;

%Save the directory of images in a variable
imagesDirectory = 'All';
%Saving the 'Gold Standard' Image in a global variable
Normal_img = imread("normal-image001.jpg");

%Cheking if the given folder exists, otherwise throw error
if ~isfolder(imagesDirectory)
    fprintf("Invalid Folder Name \n");
    return;
end

%Getting all the images with .jpg extension in file
files = fullfile(imagesDirectory, '*.jpg');
%using dir function to get lists of the files (list of images) to iterate
fileData = dir(files);

%Looping till end of the images
for i=1:length(fileData)
    Name =  fileData(i).name;
    Path = fullfile(imagesDirectory, Name);
    fprintf("%s\n",Path);
      
    image = imread(Path);
    
    %Storing the distance returned by the function for each fault
    distance = BottleMissing(image, Normal_img);
    if distance > 0.0052   %Calculating making classification with the threshold
        fprintf("No Faults - Bottle Missing\n");
    %If the bottle is present, execute the rest of checking
    else
        distance = deformed(image, Normal_img);
        if (0.0026<distance) && (distance<0.0034) %Checking threshold for deformed bottle
            fprintf("Bottle is deformed \n");
        end
        %Chceking if label is present
        distance = Label(image, Normal_img);
        if (0.0055<distance) && (distance<0.0062)
            fprintf("Label on bottle is missing \n");
        %If label is present check for its print
        elseif (0.0040<distance) && (distance<0.0045)
            fprintf("Label printing on bottle has failed \n");
        else 
            %if Label is printed then check if it is in correct position
            distance = labelStraight(image, Normal_img);
            if(0.0032<distance) && (distance<0.0060)
               fprintf("Label is positioned incorrectly (not straight) \n");
            end
        end
        %Checking if bottle is overfilled or underfilled with same crop and
        %Having different thresholds
        distance = fill_Issues(image, Normal_img);
        if (0.0035<distance) && (distance<0.00455)
            fprintf("Bottle is Overfilled \n");
        elseif (0.0048<distance) && (distance<0.0055)
            fprintf("bottle is underfilled \n")
        end
        %Checking if cap is present
        distance = cap_issues(image, Normal_img);
        if distance > 0.0057
             fprintf("Cap is missing on the bottle \n")
        end
    end

end 

%Function to check if the middle bottle is missing or not
function func = BottleMissing(image, Normal_img)
    %Cropping the region intrest from Gold Standard Image
    cropped_original_bottle = imcrop(Normal_img,[112 0 127 288]); %Imcrop function for croping as per the given dimensions
    normal_grayscale = rgb2gray(cropped_original_bottle);%Converting cropped image to gray
    
    %cropping the input image which we need to test and converting to
    %grayscale
    cropped_bottle = imcrop(image,[112 0 127 288]);
    missing_bottle = rgb2gray(cropped_bottle);
    %Storing the grayscaled normal image in vectorized histogram
    [c1, n] = imhist(normal_grayscale);
    %Normalizing the histogram of the normal image to calculate distance
    c1 = c1/size(normal_grayscale, 1) / size(normal_grayscale, 2);
    %Storing the grayscaled input image in vectorized histogram
    [c2, m] = imhist(missing_bottle);
    %Normalizing the histogram of the input image to calculate distance
    c2 = c2/size(missing_bottle,1) / size(missing_bottle,2);
    
    %Calculating euclidean distance with pdist2 function which returns
    %array of difference between the pixels of normalized image
    eudist = pdist2(c1,c2);
    %Calculating mean of the array to get a numerical threshold value
    dist = mean(eudist, 'all');
    %Returning the threshold for comparison
    func = dist;
end

%Notes to understand further
%Note - Similar approach is followed for rest of the functions, hence not
%writting repetative comments. Only the changes are in the dimensions of
%crop

%Function for finding deformed bottles
function func = deformed(image, Normal_img)
    cropped_original_bottle = imcrop(Normal_img,[150 100 100 300]);
    normal_grayscale = rgb2gray(cropped_original_bottle);

    cropped_bottle = imcrop(image,[150 100 100 300]);
    deformed_bottle = rgb2gray(cropped_bottle);

    [c1, n] = imhist(normal_grayscale);
    c1 = c1/size(normal_grayscale, 1) / size(normal_grayscale, 2);

    [c2, m] = imhist(deformed_bottle);
    c2 = c2/size(deformed_bottle,1) / size(deformed_bottle,2);

    eudist = pdist2(c1,c2);
    dist = mean(eudist, 'all');

    func = dist;
end

%Function for finding issues with labels
function func = Label(image, Normal_img)
    cropped_original_bottle = imcrop(Normal_img,[111 177 135 255]);
    normal_grayscale = rgb2gray(cropped_original_bottle);

    cropped_bottle = imcrop(image,[111 177 135 255]);
    Label = rgb2gray(cropped_bottle);

    [c1, n] = imhist(normal_grayscale);
    c1 = c1/size(normal_grayscale, 1) / size(normal_grayscale, 2);

    [c2, m] = imhist(Label);
    c2 = c2/size(Label,1) / size(Label,2);

    eudist = pdist2(c1,c2);
    dist = mean(eudist, 'all');

    func = dist;
end

%Function for finding if bottle is overfilled or underfilled
function func = fill_Issues(image, Normal_img)
    cropped_original_bottle = imcrop(Normal_img,[110 59 135 120]);
    normal_grayscale = rgb2gray(cropped_original_bottle);

    cropped_bottle = imcrop(image,[110 59 135 120]);
    Filled = rgb2gray(cropped_bottle);

    [c1, n] = imhist(normal_grayscale);
    c1 = c1/size(normal_grayscale, 1) / size(normal_grayscale, 2);

    [c2, m] = imhist(Filled);
    c2 = c2/size(Filled,1) / size(Filled,2);

    eudist = pdist2(c1,c2);
    dist = mean(eudist, 'all');

    func = dist;
end

%Function for finding if label is straigh on the bottle
function func = labelStraight(image, Normal_img)
    cropped_original_bottle = imcrop(Normal_img,[111 177 135 015]);
    normal_grayscale = rgb2gray(cropped_original_bottle);

    cropped_bottle = imcrop(image,[111 177 135 015]);
    label_straight = rgb2gray(cropped_bottle);

    [c1, n] = imhist(normal_grayscale);
    c1 = c1/size(normal_grayscale, 1) / size(normal_grayscale, 2);

    [c2, m] = imhist(label_straight);
    c2 = c2/size(label_straight,1) / size(label_straight,2);

    eudist = pdist2(c1,c2);
    dist = mean(eudist, 'all');

    func = dist;
end

%Function for finding if cap is missing or not
function func = cap_issues(image, Normal_img)
    cropped_original_bottle = imcrop(Normal_img,[108 1 138 80]);
    normal_grayscale = rgb2gray(cropped_original_bottle);

    cropped_bottle = imcrop(image,[108 1 138 80]);
    missing_Cap = rgb2gray(cropped_bottle);

    [c1, n] = imhist(normal_grayscale);
    c1 = c1/size(normal_grayscale, 1) / size(normal_grayscale, 2);

    [c2, m] = imhist(missing_Cap);
    c2 = c2/size(missing_Cap,1) / size(missing_Cap,2);

    eudist = pdist2(c1,c2);
    dist = mean(eudist, 'all');

    func = dist;
end



    
    





