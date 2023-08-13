#  Introduction: 

    In this project, we decided to work with the dataset containing a series of machining experiments were run on 2" x 2" x 1.5" wax blocks in a CNC milling machine in the System-level Manufacturing and Automation Research Testbed (SMART) at the University of Michigan. 
    Machining data was collected from the PLCs that control our CNC machines for variations in tool condition, federate, and clamp pressure used in initial machining conditions. 
    Each experiment produced a finished wax part with an "S" shape carved into the top face.

      
#  What is CNC? 

    Computer numerical control (CNC) is a manufacturing method that automates the control, movement and precision of machine tools through the use of preprogrammed computer software, which is embedded inside the tools.

    CNC is commonly used in manufacturing for machining metal and plastic parts. Mills, lathes, routers, drills, grinders, water jets and lasers are common cutting tools whose operations can also be automated with CNC. 
    It can also be used to control non-machine tools, such as welding, electronic assembly and filament-winding machines.


#  Objectives: 

    Our goals:

        1. To develop a ML model that predicts three targets
    
        3. To find insights in the various CNC measurements.


#  Description:  

    1. Type of problem: classification.


    2. Variables:

        Experiment:

              #: experiment number
  
              Material: machinable wax
  
              Tool Condition: worn tool or unworn tool
  
              Feedrate: spindle speed (mm/s)
  
              Clamp Pressure: pressure used to hold parts in place (bars)

        X1:
  
              ActualPosition: actual x position of the part (mm)
  
              ActualVelocity: actual x velocity of the part (mm/s)
  
              ActualAcceleration: actual x acceleration of part (mm/s/s)
  
              CommandPosition: reference x position of part (mm)
  
              CommandVelocity: reference x velocity of part (mm/s)
  
              CommandAcceleration: reference x acceleration of part (mm/s/s)
  
              CurrentFeedback: current (A)
  
              DCBusVoltage: voltage (V)
  
              OutputCurrent: current (A)
  
              OutputVoltage: voltage (V)
  
              OutputPower: power (in kW)

        Y1:

              ActualPosition: actual y position of part (mm)
  
              ActualVelocity: actual y velocity of part (mm/s)
  
              ActualAcceleration: actual y acceleration of the part (mm/s/s)
  
              CommandPosition: reference y position of part (mm)
  
              CommandVelocity: reference y velocity of the part (mm/s)
  
              CommandAcceleration: reference y acceleration of part (mm/s/s)
  
              CurrentFeedback: current (A)
  
              DCBusVoltage: voltage (V)
  
              OutputCurrent: current (A)
  
              OutputVoltage: voltage (V)
  
              OutputPower: power (kW)

        Z1:

              ActualPosition: actual z position of the part (mm)
  
              ActualVelocity: actual z velocity of the part (mm/s)
  
              ActualAcceleration: actual z acceleration of the part (mm/s/s)
  
              CommandPosition: reference z position of part (mm)
  
              CommandVelocity: reference z velocity of the part (mm/s)
  
              CommandAcceleration: reference  z acceleration of part (mm/s/s)
  
              CurrentFeedback: current (A)
  
              DCBusVoltage: voltage (V)
  
              OutputCurrent: current (A)
  
              OutputVoltage: voltage (V)

        S1:

              ActualPosition: actual position of spindle (mm)
  
              ActualVelocity: actual velocity of spindle (mm/s)
  
              ActualAcceleration: actual acceleration of spindle (mm/s/s)
  
              CommandPosition: reference position of spindle (mm)
  
              CommandVelocity: reference velocity of spindle (mm/s)
  
              CommandAcceleration: reference acceleration of spindle (mm/s/s)
  
              CurrentFeedback: current (A)
  
              DCBusVoltage: voltage (V)
  
              OutputCurrent: current (A)
  
              OutputVoltage: voltage (V)
  
              OutputPower: current (A)
  
              SystemInertia: torque inertia (kg*m^2)

        M1:
  
              M1_CURRENT_PROGRAM_NUMBER: number the program is listed under on the CNC
  
              M1_sequence_number: line of G-code being executed
  
              M1_CURRENT_FEEDRATE: spindle speed (mm/s)

        Machining:

              Process: current machining stage is being performed. Includes preparation, tracing up and down the "S" curve involving different layers, and repositioning of the spindle as it moves through the air to a certain starting point
  
              Finalizes: machining process able to complete without the part moving out of the pneumatic vise
  
              Visual Inspection: The finished part had adequate geometry and surface finish. An experiment that fails to finalize will have "NA"


    3. Target:
  
          1.tool_condition
  
          2.machining_finalized
  
          3.passed_visual_inspection


    4. Steps We made

          1. Encode Machining_Process
  
          2. Train / Test split:
  
                > Train - 77%
    
                > Test - 33%
  
          3. Scaling
  
                > StandardScaler
  
          4. Balancing:
  
                > RandomOverSampler
    
                > RandomUnderSampler
  
          5. Training Classification Algorithms
  
          6. Fine-tuning hyperparameters:
  
                > RanomizedSearchCV
  
          7. Model evaluation and result analysis

          

    5. ML Algorithms we used:

          1. RandomForest Classifier
  
          2. GradientBoost Classifier  
  
          3. DecistionTree Classifier
  
          4. KNeighbors Classifier
  
          5. XGB Classifier

 

    6. Evaluation metrix we used: 

          1. Confusion Matrix
  
          2. Roc-auc
  
          3. Precision
  
          4. Recall
  
          5. Accuracy


# Conclusion

##  Data Analysis

    After completing the data analysis step, we found several insights. 

          1. the amount of labels 'Starting' and 'end' in variable 'Machining_Process' is relatively small and thus creates outliers
  
          2. More than half of the samples were done with feedrate 3, while almost half of the samples had 4.0 pressure on clamp
  
          3. feedrate on worn machines has more variation like 12 and 15 which are being abset on unworn tools, but feedrate 3 is still the most common one, taking more than 50% on both worn and unworn tools.
  
          4. clamp_pressure seems to not change significantly and has almost the same counts on both worn and unworn tools. 
  
          5. nearly half of the experiment was made with worn tools, while the other half with unworn 
  
          6. more than 90% of experiments were successfully finished by the machine
  
          7. more than 75% of the experiment passed the visual inspection by human
  
          8. the count of details that didn't pass visual inspection by humans is higher than the amount of details that didn't pass the machining process. We assume that is because humans have some tricks and techniques to detect failures and waste, which machines can't do.
  
          9. Count of 'Not Finalized Machining' with worn tools is almost the same as that with unworn tools. It means that regardless of the tool condition, the detail will be finished by the machine.
  
          10.Count of 'Not Passed Visual insepction with worn tool is relatively larger than that with unworn tool. 
  
          11.We assume that the tool condition does affect the process of machining itself, but not dramatically as failure and waste can occur on unwarn tools as well, but in smaller proportions and unlike machines, humans can notice the difference affected by warn tools. 
  
          12.'Z1_CurrentFeedback', 'Z1_DCBusVoltage', 'Z1_OutputCurrent', 'Z1_OutputVoltage', 'Z1_SystemInertia' are constant variables
  
          13.There are no null values in the dataset.
  
          14.tool_condition seems to be relatively balanced
  
          15.machining_finalized seems to be  not balanced at all
  
          16.passed_visual_inspection seems to be not balanced

        
        
   ## Training & Evaluating models
  
     > We worked with three different targets and applied all the steps below to each individual target.

     > We have tested 3 methods of balancing the dataset

     > We have tested 5 popular models with the same data and same preprocessing steps to then compare them and choose only fixed amounts to tune the hyperparameters. 

     > After testing 5 models, we chose only 3 best models according to the accuracy metrics 

     > Then, we used cross-validation and started to fine-tune hyperparameters to boost the performance 

     > lastly, We have evaluated the models we built using several metrics including: 

           1. F1 score
  
           2. Roc-auc 
  
           3. Confusion Matrix 
  
           4. Precision
  
           5. Recall 
  
           6. Accuracy

         
    By analyzing all metrics we can tell that: 

          1. We need to encode the Machining_Process
  
          2. It seems that the models trained on data balanced by RandomOverSampler give the best performances without any causes of overfitting for predicting Machining_process
  
          3. It seems that the model works equally well on both balanced and not balanced datasets and gives the highest performance without any causes of overfitting for predicting passed_visual_inspection.
  
          4. Even after fine-tuning of hyperparameters by RandomSearchCV, the results of models are almost the same. We assume that tuning the hyperparameters is not necessary in this project, because most models give almost 100% of performance by default settings.
  
          5. According to the matrixes we used (Precistion, Recall, Accuracy and ROC_AUC), the best choice for predicting tool_condition is the RandomForestClassifier, because it gives the best performance without any causes of overfitting.
  
          6. According to the matrixes we used (Precistion, Recall, Accuracy and Roc_auc), we can use any of the models we have tested in this project (RandomForestClassifier, XGBoostClassifier and DecisionTreeClassifier) for predicting machining_finalized, because all of them give the best performances without any causes of overfitting.
  
          7. According to the matrixes we used (Precistion, Recall, Accuracy and Roc_auc), we can use any of the models we have tested in this project (RandomForestClassifier, XGBoostClassifier and DecisionTreeClassifier) for predicting passed_visual_inspection, because all of them give the best performances without any causes of overfitting.

        

   # THANKS!!! :)
