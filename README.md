# lstm-parser
Transition based dependency parser with state embeddings computed by LSTM RNNs and training with exploration as presented in this EMNLP 2016 paper: http://www.aclweb.org/anthology/D/D16/D16-1211.pdf

See https://github.com/clab/lstm-parser for information about embeddings and data.


# Build instructions

    mkdir build
    cd build
    cmake -DEIGEN3_INCLUDE_DIR=/opt/tools/eigen-dev ..
    (in allegro: cmake -DEIGEN3_INCLUDE_DIR=/opt/tools/eigen-dev -G 'Unix Makefiles')
    make -j2

    
# Command to run the parser (in allegro): 

    parser/lstm-parse -h
    
    parser/lstm-parse -T train.txt -d dev.txt --hidden_dim 100 --lstm_input_dim 100 -w sskip.100.vectors --pretrained_dim 100 --rel_dim 20 --action_dim 20 -t -P
    
# How to get the arc-std oracles (traning set and dev set of the parser) having a CoNLL 2006 file:
   
    java -jar ParserOracleArcStd.jar -t -1 -l 1 -c train10.conll -i train10.conll > oracleTrainArcStd.txt
    (the train10.conll file should be fully projective)
    


