<template>
    <v-card align="center" >
        <v-col cols="6" align="left">
            <a href="./#/">
            Go back
            </a><br><br>
            <h1>Meet TAMI</h1>
            <p> We work with <a href="www.openai.com">openAI.com</a> to create code from natural language instructions (or voice commands) that would
            build and <strong>deploy</strong> machine learning models for your business without the need to know anything about IT, or own any infrastructure.
                Here's an instruction example: <br><br>
                [me] - "Tami", please create and deploy a machine learning model that will predict the price of energy in October 2023<br><br>

                [Answer from Tami] - Ok please give me a a few minutes to perform all the tasks....done! The predicted price for 
                October 2023 is $200 per KW/h. Here's the generated code<br><br>
                <v-alert
                color="success"
                icon="$success"
                title="Code Generated"
                text="">
                import os<br>
                import sagemaker<br>
                from sagemaker import get_execution_role<br>
                from sagemaker.tensorflow import TensorFlow<br>
                import boto3<br>
                from sagemaker import image_uris<br>
                from sagemaker.session import Session<br>
                from sagemaker.inputs import TrainingInput<br>
                import json<br><br>

                sagemaker_session = sagemaker.Session()<br>
                role = get_execution_role()<br>
                region = sagemaker_session.boto_session.region_name<br>

                hyperparameters = {<br>
                     "max_depth":"5",<br>
                     "eta":"0.2",<br>
                     "gamma":"4",<br>
                     "min_child_weight":"6",<br>
                     "subsample":"0.7",<br>
                     "objective":"reg:squarederror",<br>
                     "num_round":"50",<br>
                     "objective":"binary:logistic",}<br>
                
                bucket = 's3://model-inputs'<br>
                output_path = 's3://model-inputs'<br>

                xgboost_container = sagemaker.image_uris.retrieve(framework='xgboost', region='eu-central-1', version='latest')<br>

                estimator = sagemaker.estimator.Estimator(image_uri=xgboost_container, <br>
                                          hyperparameters=hyperparameters,<br>
                                          role=sagemaker.get_execution_role(), <br>
                                          instance_count=1, <br>
                                          instance_type='ml.m5.2xlarge', <br>
                                          volume_size=5, # 5 GB <br> 
                                          output_path=output_path) <br>
                
                content_type = "text/csv"<br>
                train = sagemaker.inputs.TrainingInput(s3_data='s3://model-inputs/train_model_inputs.csv', content_type=content_type)<br>
                estimator.fit({'train': train, 'validation': val})



                </v-alert>
            </p>

            
        </v-col>
    </v-card>    
</template>

<script>
