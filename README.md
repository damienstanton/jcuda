### To install

Tested with _OS X 10.10, Xcode 7, CUDA 7.5 or newer, OpenCL 1.2 or newer_

First, fix a problem with Xcode not recognizing CUDA framework

```sh
sudo install_name_tool -change @rpath/CUDA.framework/Versions/A/CUDA \
    /Library/Frameworks/CUDA.framework/CUDA \
    /usr/local/cuda/lib/libcuda.dylib
```

Then generate the build files, compile and copy the libraries to a location on your java.library.path. You can find this with `java -XshowSettings:properties`

```sh
cd jcuda-main
mkdir build && cd build
cmake ..
# -j should be your number of cores
make -j4
# since there is no make install directive you need to copy the dylibs directly
sudo cp -R ./lib /Library/Java/Extensions 
```

Finally build and package the JARs, run unit tests and generate Javadocs
```sh
cd ..
# tests fail due to a problem with linking/java.library.path, so skip them
mvn clean package -DskipTests
```

All JARfiles should build except the last one, jcuda-main. This is expected.

Check that from the root JCUDA directory that `find ./ -name *.jar` returns

```sh
.//jcublas/JCublasJava/target/jcublas-0.7.0b-SNAPSHOT-javadoc.jar
.//jcublas/JCublasJava/target/jcublas-0.7.0b-SNAPSHOT-sources.jar
.//jcublas/JCublasJava/target/jcublas-0.7.0b-SNAPSHOT.jar
.//jcuda/JCudaJava/target/jcuda-0.7.0b-SNAPSHOT-javadoc.jar
.//jcuda/JCudaJava/target/jcuda-0.7.0b-SNAPSHOT-sources.jar
.//jcuda/JCudaJava/target/jcuda-0.7.0b-SNAPSHOT.jar
.//jcufft/JCufftJava/target/jcufft-0.7.0b-SNAPSHOT-javadoc.jar
.//jcufft/JCufftJava/target/jcufft-0.7.0b-SNAPSHOT-sources.jar
.//jcufft/JCufftJava/target/jcufft-0.7.0b-SNAPSHOT.jar
.//jcurand/JCurandJava/target/jcurand-0.7.0b-SNAPSHOT-javadoc.jar
.//jcurand/JCurandJava/target/jcurand-0.7.0b-SNAPSHOT-sources.jar
.//jcurand/JCurandJava/target/jcurand-0.7.0b-SNAPSHOT.jar
.//jcusolver/JCusolverJava/target/jcusolver-0.7.0b-SNAPSHOT-javadoc.jar
.//jcusolver/JCusolverJava/target/jcusolver-0.7.0b-SNAPSHOT-sources.jar
.//jcusolver/JCusolverJava/target/jcusolver-0.7.0b-SNAPSHOT.jar
.//jcusparse/JCusparseJava/target/jcusparse-0.7.0b-SNAPSHOT-javadoc.jar
.//jcusparse/JCusparseJava/target/jcusparse-0.7.0b-SNAPSHOT-sources.jar
.//jcusparse/JCusparseJava/target/jcusparse-0.7.0b-SNAPSHOT.jar
```

And you are done!