# SpinningUp with Docker on VS-Code

## Install
```
git clone https://www.github.com/cj-l/spinningup-docker
cd spinningup-docker
code .
```
Reopen in container
- will install spinningup and other dependencies

Run the following commands to enable watching videos of trained agents
```
export PYTHONPATH="${PYTHONPATH}:`pwd`"
Xvfb :1 -screen 0 1024x768x24 -ac +extension GLX +render -noreset &> xvfb.log &
export DISPLAY=:1
```

## Test the installation

Train PPO
```
python -m spinup.run ppo --hid "[32,32]" --env LunarLander-v2 --exp_name installtest --gamma 0.999
```
Watch a video
```
python -m spinup.run test_policy data/installtest/installtest_s0
```
