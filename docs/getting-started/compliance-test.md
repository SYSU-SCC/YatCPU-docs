# 配置 RISC-V 合规性测试

本节提供新旧两种RISC-V合规性测试的途径，这两种途径的测试效果是相似的。新框架测试（riscof）生成的测试报告更完整美观，旧框架测试的环境配置更简单一些。

## 使用 riscof 测试

新框架测试目前仅支持在 Linux 环境中运行。

### 安装必要组件

为了进行新框架的测试，我们需要安装的必要组件如下：

- 1.Python3.6  pip3 
- 2.RISCOF   
- 3.RISCV-GNU Toolchain   
- 4.spike   
- 5.Architectual Tests

1.Python3.6  pip3 

请自行安装Python3.6以及pip3

2.RISCOF

```
pip3 install git+https://github.com/riscv/riscof.git
```

3.RISCV-GNU Toolchain 

```
sudo apt install binutils-riscv64-unknown-elf gcc-riscv64-unknown-elf
```

4.spike

```
sudo apt-get install device-tree-compiler
git clone https://github.com/riscv-software-src/riscv-isa-sim.git
cd riscv-isa-sim
mkdir build
cd build
../configure --prefix=/usr/local
make
sudo make install
```

5.Architectual Tests

下面的命令会在将Architectual Tests克隆到当前文件夹
```
riscof --verbose info arch-test --clone
```

### 编译仿真器代码

参考[波形仿真一节](simulation.md#%E4%BD%BF%E7%94%A8-verilator-%E4%BB%BF%E7%9C%9F%E7%94%9F%E6%88%90%E6%B3%A2%E5%BD%A2%E6%96%87%E4%BB%B6)先编译出仿真器可执行程序。

  
### 运行测试

```
cd riscof-target
riscof --verbose info run --config ./config.ini --no-browser --suite YOURPATH/riscv-arch-test/riscv-test-suite/rv32i_m \
--env YOURPATH/riscv-arch-test/riscv-test-suite/env
```

测试通过后会生成一个 HTML 格式的报告。报告位于riscof-target/rsicof_work/report.html

### 关于riscof

更多关于新测试框架riscof的资料，请参考官方文档<https://riscof.readthedocs.io/en/latest/index.html>

## 旧框架测试

RISC-V 合规性测试仅支持使用 Makefile 与 gcc 编译器运行，如果你使用的是 Windows，那么你应当配置好 mingw 环境，以便运行测试。如果你已经配置好 Verilator，那么合规性测试应当也可以正常运行。

### 编译仿真器代码

参考[波形仿真一节](simulation.md#%E4%BD%BF%E7%94%A8-verilator-%E4%BB%BF%E7%9C%9F%E7%94%9F%E6%88%90%E6%B3%A2%E5%BD%A2%E6%96%87%E4%BB%B6)先编译出仿真器可执行程序。

### 克隆合规性测试仓库

合规性测试仓库位于 [https://github.com/riscv-non-isa/riscv-arch-test](https://github.com/riscv-non-isa/riscv-arch-test)，旧框架是该仓库的一个分支。在和yatcpu同级（注意不要克隆到yatcpu内）处运行下面的命令。

```
git clone -b old-framework-2.x https://github.com/riscv-non-isa/riscv-arch-test.git
```

克隆完成后，进入合规性测试仓库，配置环境变量，并编译运行测试：

```bash
cd riscv-arch-test
export TARGET_SIM=~/yatcpu/verilog/verilator/obj_dir/VTop
export TARGETDIR=~/yatcpu/riscv-target
export RISCV_TARGET=yatcpu
make
```
