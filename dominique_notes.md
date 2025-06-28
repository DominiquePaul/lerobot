Leader: /dev/tty.usbmodem59700724381
Follower: /dev/tty.usbmodem59700741781

# Calibration

**Leader arm**

python -m lerobot.calibrate \
    --teleop.type=so100_leader \
    --teleop.port=/dev/tty.usbmodem59700724381 \
    --teleop.id=doms_leader_arm

**Follower arm**

python -m lerobot.calibrate \
    --robot.type=so100_follower \
    --robot.port=/dev/tty.usbmodem59700741781 \
    --robot.id=doms_follower_arm


# Teleoperate

python -m lerobot.teleoperate \
    --robot.type=so100_follower \
    --robot.port=/dev/tty.usbmodem59700741781 \
    --robot.id=doms_follower_arm \
    --teleop.type=so100_leader \
    --teleop.port=/dev/tty.usbmodem59700724381 \
    --teleop.id=doms_leader_arm

### With camera

python -m lerobot.teleoperate \
    --robot.type=so100_follower \
    --robot.port=/dev/tty.usbmodem59700741781 \
    --robot.id=doms_follower_arm \
    --robot.cameras="{ context: {type: opencv, index_or_path: 1, width: 1920, height: 1080, fps: 30}, arm: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 30}}" \
    --teleop.type=so100_leader \
    --teleop.port=/dev/tty.usbmodem59700724381 \
    --teleop.id=doms_leader_arm \
    --display_data=true



# Record data

export DATASET_NAME=chess_movements
export DESCRIPTION="Move the piece from red to blue"

python -m lerobot.record \
    --robot.type=so100_follower \
    --robot.port=/dev/tty.usbmodem59700741781 \
    --robot.id=doms_follower_arm \
    --robot.cameras="{ context: {type: opencv, index_or_path: 1, width: 1920, height: 1080, fps: 30}, arm: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 30}}" \
    --teleop.type=so100_leader \
    --teleop.port=/dev/tty.usbmodem59700724381 \
    --teleop.id=doms_leader_arm \
    --display_data=true \
    --dataset.repo_id=${HF_USER}/${DATASET_NAME} \
    --dataset.num_episodes=2 \
    --dataset.single_task=${DESCRIPTION} \
    --dataset.num_image_writer_processes 8


### Mock robot

python -m lerobot.record \
    --robot.type=mock_robot \
    --robot.cameras="{ context: {type: opencv, index_or_path: 1, width: 1920, height: 1080, fps: 30}, arm: {type: opencv, index_or_path: 0, width: 1920, height: 1080, fps: 30}}" \
    --teleop.type=mock_teleop \
    --display_data=true \
    --dataset.repo_id=${HF_USER}/${DATASET_NAME} \
    --dataset.num_episodes=2 \
    --dataset.single_task=${DESCRIPTION}