# Health-and-Wellness-Tracker
# Daily Management tool used to flag abnormalities in people's health, motivate people to be aware of their overall health, and prevent detrimental effects of chronic diseases.
clc; 
clear;
# Introduction, where the patient chooses what he wants to calculate.
n = 1;
while n == 1
    q1= fprintf('Welcome to the health tracker calculator, please select which option you are interested in knowing more about: \n  ');
    options1= fprintf(' A) Body Mass Index     B) Heart Rate \n   C) Water Intake        D) Sleep Tracker\n');
    ans1= input('Option: ','s');
   
    % BMI
    if ans1 == 'A'
    height = input('Enter your height in meters: ');
        weight1 = input('Enter your weight in pounds: ');
        weight = weight1/2.205;
        BMI = weight/(height^2);
        fprintf('Your BMI is: %.2f\n' , BMI);
        if BMI<18.5
            UW = 'Underweight: Contact a nutritionist regarding a meal plan that can help you increase your calorie intake. ';
            msgbox(UW, 'Diagnosis');
        elseif BMI>=18.5 && BMI < 25
            IW = 'Ideal weight: Try to remain within these values';
            msgbox(IW, 'Diagnosis');
        elseif BMI >=25 && BMI < 30
            OW = 'Overweight: Contact a nutritionist regarding a meal plan that can help you decrease your calorie intake. Remember to add an exercise routine to your daily health plan.';
            msgbox(OW, 'Diagnosis');
        else
            OB = 'Obese: Contact a nutritionist regarding a meal plan that can help you decrease your calorie intake. Remember to add an exercise routine to your daily health plan.';
            msgbox(OB, 'Diagnosis');
        end
        fprintf('Here is an image that explains the Body Mass Index Chart \n')
        pause(4)
   
        im = imread('bmi-chart_1-1.jpg');
        figure
        imshow(im)
       
        loop = input('Do you want to go back to the main menu: ','s');
        if strcmp(loop,'yes')
            n = 1;
            fprintf('\n')
            continue
        else
            n = 0;
            continue
        end
    end
   
    % Resting Heart Rate
    if ans1=='B'
       fprintf('Count number of palpitations in 15 seconds\n')
        palp= input('Enter Heart Palpitations 15 seconds: ');
        HR= palp*4;
        Age = input('Enter your age (must be 20 or older): ');
        data = readtable('Heart Rate.csv');
        var1Array = data{:,'Var1'};
        var2Array = data{:,'Var2'};
        var3Array = data{:,'Var3'};
        matrix=[var1Array,var2Array,var3Array];
        x1 = matrix(Age-19,2);
        x2 = matrix(Age-19,3);
        if HR < x1
            hypo='Your heart rate is below the normal range, for your age.';
            msgbox(hypo, 'Diagnosis');
        elseif HR> x2
            hyper='Your heart rate is above the normal range, for your age.';
            msgbox(hyper, 'Diagnosis');
        else
            good='Congratulations! You have a healthy heart rate!';
            msgbox(good, 'Diagnosis');
        end
       
        pause(2)
        fprintf('Here is a graph that shows the target heart rate zone for your age. \n')
        line=x1:x2;
        plot(line,zeros(size(line)),'k-','LineWidth', 4,'Color','b')
        hold on
        plot(x1,0,'|','MarkerSize',9,'MarkerEdgeColor','b','LineWidth',3)
        hold on
        plot(x2,0,'|','MarkerSize',9,'MarkerEdgeColor','b','LineWidth',3)
        hold on
        plot(HR,0,'X','MarkerSize',12','LineWidth', 3,'Color','r')
        yticks([]);
        xlim([60,190])
        xlabel('Heart Rate (bpm)')
        title('Target Heart Rate')

        loop2 = input('Do you want to go back to the main menu: ','s');
        if strcmp(loop2,'yes')
            n = 1;
            fprintf('\n')
            continue
        else
            n = 0;
            continue
        end

    end
   
    % Water Intake
    if ans1=='C'
      pounds= input('Enter your weight in pounds: ');
      h2o= input('Enter the amount of water cups you''ve had today: ');
        data2 = readtable('Water Intake.csv');
        var1Array2 = data2{:,'Var1'};
        var2Array2 = data2{:,'Var2'};
   
        matrix2=[var1Array2,var2Array2];
        x11 = matrix2(pounds,2);
   
        if x11<h2o
           more ='You are drinking more water than needed';
           msgbox(more, 'Diagnosis');
        elseif x11>h2o
            less= 'You need to drink more water';
            msgbox(less, 'Diagnosis');
        else
            norm= 'You are drinking the suggested amount of water';
            msgbox(norm, 'Diagnosis');
        end
   
        pause(3)
        fprintf('Here is a graph that shows the amount of water intake in cups depending on your weight. \n')
       
        x = 80:10:250;
        y = [5, 6, 6, 7, 7, 8, 9, 9, 10, 11, 11, 12, 12,13,14,14,15,16];
        figure
        bar(x,y)
        hold on
        plot(pounds,x11,'X','MarkerSize',12','LineWidth', 3,'Color','r')
        xlabel('Weight (lb)')
        ylabel('8oz cups of water')
        title('Water Intake')

        loop3 = input('Do you want to go back to the main menu: ','s');
        if strcmp(loop3,'yes')
            n = 1;
            fprintf('\n')
            continue
        else
            n = 0;
            continue
        end

    end
   
    % Sleep Tracker
    if ans1 == 'D'
        fprintf('Research shows that a healthy individual requires between 7 and 9 hours of sleep \n')
        pause(2)
        sleep = input('Enter hours of sleep received last night: ');
        if sleep<=9
            percentsleep = sleep*100/9;
            rest = 100 - percentsleep;
            percentages = [percentsleep, rest];
            colors = [0 1 0; 1 0 0];
            piechart = pie(percentages);
            title('Sleep Tracker');
            colormap(colors)
            total = sum(percentages);
            percentages_text = cellstr(num2str(percentages(:)./total*100,'%0.1f%%'));
            textObjs = findobj(piechart,'Type','text');
            for i = 1:numel(percentages_text)
                textObjs(i).String = percentages_text{i};
            end
            legend('Amount of sleep received', 'Sleep needed');
        else
            sleep = 'You are sleeping more than the hours recommended to normally function during the day. It is recommended to sleep from 7 - 9 hours.';
            msgbox(sleep, 'Diagnosis');
        end

        loop4 = input('Do you want to go back to the main menu: ','s');
        if  strcmp(loop4,'yes')
            n = 1;
            fprintf('\n')
            continue
        else
             n = 0;
            continue
        end
    end
end
