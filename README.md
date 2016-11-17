# lstm-parser
Transition based dependency parser with state embeddings computed by LSTM RNNs and training with exploration as presented in this EMNLP 2016 paper: http://www.aclweb.org/anthology/D/D16/D16-1211.pdf

See https://github.com/clab/lstm-parser for more information (https://github.com/clab/lstm-parser is the parser with static oracles presented in ACL 2015)


#### Required software

 * A C++ compiler supporting the [C++11 language standard](https://en.wikipedia.org/wiki/C%2B%2B11)
 * [Boost](http://www.boost.org/) libraries
 * [Eigen](http://eigen.tuxfamily.org) (newer versions strongly recommended)
 * [CMake](http://www.cmake.org/)
 * [gcc](https://gcc.gnu.org/gcc-5/) (only tested with gcc version 5.3.0, may be incompatible with earlier versions)

#### Build instructions

    mkdir build
    cd build
    cmake .. -DEIGEN3_INCLUDE_DIR=/path/to/eigen
    make -j2

#### Train a parsing model

Having a training.conll file and a development.conll formatted according to the [CoNLL data format](http://ilk.uvt.nl/conll/#dataformat), to train a parsing model with the LSTM parser type the following at the command line prompt:

    java -jar ParserOracleArcStd.jar -t -1 -l 1 -c training.conll > trainingOracle.txt
    java -jar ParserOracleArcStd.jar -t -1 -l 1 -c development.conll > devOracle.txt

    parser/lstm-parse -T trainingOracle.txt -d devOracle.txt --hidden_dim 100 --lstm_input_dim 100 -w sskip.100.vectors --pretrained_dim 100 --rel_dim 20 --action_dim 20 -t -P
    
Link to the word vectors that we used in the ACL 2015 paper for English:  [sskip.100.vectors](https://drive.google.com/file/d/0B8nESzOdPhLsdWF2S1Ayb1RkTXc/view?usp=sharing).

Note-1: you can also run it without word embeddings by removing the -w option for both training and parsing.

Note-2: the training process should be stopped when the development result does not substantially improve anymore. Normally, after 5500 iterations.

Note-3: the parser reports (after each iteration) results including punctuation symbols while in the ACL-15 and the EMNLP-16 paper we report results excluding them (as it is common practice in those data sets). You can find eval.pl script from the CoNLL-X Shared Task to get the correct numbers.

#### Parse data with your parsing model

Having a test.conll file formatted according to the [CoNLL data format](http://ilk.uvt.nl/conll/#dataformat)

    java -jar ParserOracleArcStd.jar -t -1 -l 1 -c test.conll > testOracle.txt

    parser/lstm-parse -T trainingOracle.txt -d testOracle.txt --hidden_dim 100 --lstm_input_dim 100 -w sskip.100.vectors --pretrained_dim 100 --rel_dim 20 --action_dim 20 -P -m parser_pos_2_32_100_20_100_12_20-pidXXXX.params

The model name/id is stored where the parser has been trained.
The parser will output the conll file with the parsing result.

#### Citation

If you make use of this software, please cite the following:

    @InProceedings{ballesteros-EtAl:2016:EMNLP2016,
      author    = {Ballesteros, Miguel  and  Goldberg, Yoav  and  Dyer, Chris  and  Smith, Noah A.},
      title     = {Training with Exploration Improves a Greedy Stack LSTM Parser},
      booktitle = {Proceedings of the 2016 Conference on Empirical Methods in Natural Language Processing},
      month     = {November},
      year      = {2016},
      address   = {Austin, Texas},
      publisher = {Association for Computational Linguistics},
      pages     = {2005--2010},
      url       = {https://aclweb.org/anthology/D16-1211}
    }

#### License

This software is released under the terms of the [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0).

#### Contact

For questions and usage issues, please contact miguelballesteros1@gmail.com and yoav.goldberg@gmail.com
