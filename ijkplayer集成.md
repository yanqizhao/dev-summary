# ijkplayer集成

1. 拉取ijkplayer最新tag的代码k0.8.29并运行所需脚本

    * git clone https://github.com/Bilibili/ijkplayer.git ijkplayer-ios
    * cd ijkplayer-ios
    * git checkout -B latest k0.8.29

    * ./init-ios.sh
    * ./init-ios-openssl.sh

    * cd ios
    * ./compile-ffmpeg.sh clean
    * ./compile-ffmpeg.sh all

2. 打包

    * carthage build --no-skip-current
    * carthage archive

