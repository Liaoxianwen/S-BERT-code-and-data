# S-BERT-code-and-data
S-BERT code and data (Vietnamese typical event sentences and Vietnamese text classification dataset) for the paper "Efficient Estimate of Sentence's Representation Based on the Difference Semantics Model".

Environmental requirements for windows：Anaconda3, Tensorflow 1.12 gpu + python 3.6

Usage：

  (1) Download the basic BERT from https://github.com/google-research/bert, unzip the compressed package, modify the following code in run_pretraining_model1_v2.py :
  
  
          flags.DEFINE_string(
            "bert_config_file", "bert-vietnamese-L_12-H-768/multi_cased_L-12_H-768_A-12/bert_config.json",
            "The config json file corresponding to the pre-trained BERT model. "
            "This specifies the model architecture.")

        flags.DEFINE_string(
            "input_file", "vi_sents4trainBERT_4_model_1",  # the dataset used to fine-tune S-BERT on the basis of BERT
            "Input TF example files (can be a glob or comma separated).")

        flags.DEFINE_string(
            "output_dir", "pretrain_output",
            "The output directory where the model checkpoints will be written.")

        ## Other parameters
        flags.DEFINE_string(
            "init_checkpoint", "bert-vietnamese-L_12-H-768/multi_cased_L-12_H-768_A-12/bert_model.ckpt", #chinese_L-12_H-768_A-12/bert_model.ckpt
            "Initial checkpoint (usually from a pre-trained BERT model).")
            
   (2) Create dataset used to fine-tuned S-BERT
       A) Prepare a text file. Each line in the text file is a sentence, separated by a blank line between paragraphs. If you are downloading data from Wikidata, first you need to extract the article correctly, and then use the tool to split each paragraph into separate sentences. Important note: Each line in this text file represents a sentence,           and paragraphs are separated by blank lines. 
       B) Create the dataset which will be used to fine-tuned S-BERT. The python file used to create the dataset is create_pretraining_data_4_model1.py
          The following code in create_pretraining_data_4_model1.py may be modified:
          
            # flags.DEFINE_string("input_file", None,"Input raw text file (or comma-separated list of files).")
            flags.DEFINE_string("input_file", "data_dir\\dic_expl\\dic_expl_4trainBert.txt",  # the text file created in the step "(2)-->A)".
                      "Input raw text file (or comma-separated list of files).")

            # flags.DEFINE_string("output_file", None,"Output TF example file (or comma-separated list of files).")
            flags.DEFINE_string("output_file", "vi_sents4trainBERT_4_model_1", "Output TF example file (or comma-separated list of files).")

            # flags.DEFINE_string("vocab_file", None,"The vocabulary file that the BERT model was trained on.")
            flags.DEFINE_string("vocab_file", "bert-vietnamese-L_12-H-768/multi_cased_L-12_H-768_A-12/vocab.txt",
                      "The vocabulary file that the BERT model was trained on.")

            flags.DEFINE_bool(
            "do_lower_case", False,  # very important. This flag should be consistent with the model. For example, when the pre-trained BERT is case sensitive, fill in False here. 
            "Whether to lower case the input text. Should be True for uncased "
            "models and False for cased models.")
            
        * * After setting the corresponding parameters, run create_pretraining_data_4_model1.py to create the dataset.
        
(3) Fine-tune S-BERT. The code used to fine-tune S-BERT is in the python file run_pretraining_model1_v2.py. Modify parameters shown in step (1) and run this file to fine-tune. "epoch" is set to 1.5. You can also set the "epoch" according to your own situation. 
    
(4) When S-BERT has been fine-tuned in step (3), we can use S-BERT to calcuate the semantics vectors of sentences.  The python code used to calculate the semantics vectors of sentences is in get_Sent_BERT_sentence_Vec.py.
    The following code in the file may be modified.

                input_filename = "H:\\2th_English_Vi_Experiment\\cls_xnli_data\\viNews\\viNews_2th.txt" # Sentences to be calculated. One line in this file is a sentence.
                flags = tf.flags

                FLAGS = flags.FLAGS

                ## Required parameters
                flags.DEFINE_string(
                "data_dir", 'data_dir',
                "The input data dir. Should contain the .tsv files (or other data files) "
                "for the task.")

                flags.DEFINE_string(
                "bert_config_file", "bert-vietnamese-L_12-H-768/multi_cased_L-12_H-768_A-12/bert_config.json",
                "The config json file corresponding to the pre-trained BERT model. "
                "This specifies the model architecture.")

                flags.DEFINE_string("task_name", "getsentsvec", "The name of the task to train.")

                flags.DEFINE_string("vocab_file", "bert-vietnamese-L_12-H-768/multi_cased_L-12_H-768_A-12/vocab.txt",
                            "The vocabulary file that the BERT model was trained on.")

                flags.DEFINE_string(
                "output_dir", "output_dir",
                "The output directory where the model checkpoints will be written.")

                ## Other parameters

                flags.DEFINE_string(
                "init_checkpoint", "H:\\2th_English_Vi_Experiment\\vi_pretrained_S-BERT-res\\model.ckpt.index",
                "Initial checkpoint (usually from a pre-trained BERT model).")

                flags.DEFINE_bool(
                "do_lower_case", False,  # very important. This flag should be consistent with the model.
                "Whether to lower case the input text. Should be True for uncased "
                "models and False for cased models.")
                
                ....
                output_predict_file = os.path.join(FLAGS.output_dir, input_filename+".Sent-BERT_results.tsv") # The semantics vector file, one vector per line.
                
=====================
This code is currently not particularly friendly. So you need to have TensorFlow 1.x and python programming skills. We will improve the ease of use of this code in the future. 
    
       
          
          
           
