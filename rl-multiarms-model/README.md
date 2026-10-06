## Setup

Python 3.7 dependencies:
 - PyTorch 1.6.0
 - pybullet
 - numpy
 - numpy-quaternion
 - ray
 - tensorboardX

Trong file conda YAML có chứa tất cả những tham số cần thiết cho chương trình. Để sử dụng nó, chạy câu lệnh dưới đây:

```sh
conda env create -f environment.yml
conda activate multiarm
```

## Xác định mô hình lập kế hoạch chuyển động ban đầu | Evaluate the pretrained motion planner

Tải xuống bộ trọng số huấn luyện model và tính toán lại hiệu suất hoạt động cho model tại đường dẫn dưới đây:
```sh
wget -qO- https://multiarm.cs.columbia.edu/downloads/checkpoints/ours.tar.xz | tar xvfJ -
wget -qO- https://multiarm.cs.columbia.edu/downloads/data/benchmark.tar.xz | tar xvfJ -
```
Bước tính toán hiệu suất cho model trên bộ trọng số huấn luyện và chế độ tĩnh của robot như sau:
```sh
python main.py --mode benchmark --tasks_path benchmark/ --load ours/ours.pth --num_processes 1 --gui
```
Loại bỏ dòng lệnh `--gui` để chạy tự động, có thể sử dụng thêm CPU cho quá trình huấn luyện bằng cách thay đổi `--num_processes 16`.

Để hiển thị kết quả đánh giá hiệu suất
```sh
python summary.py ours/benchmark_score.pkl
```
Để tính toán hiệu suất của bộ trọng số pretrain phía trên cho hệ thống robot động, chạy lệnh: 
```sh
python benchmark_dynamic.py --mode benchmark --tasks_path benchmark/ --load ours/ours.pth --num_processes 1 --gui
```

## Huấn luyện mô hình lập kế hoạch chuyển động phân tán cho hệ đa cánh tay robot 

Trong đường dẫn dưới đây, tải xuống mô hình huấn luyện cho các nhiệm vụ robot cần thực hiện và bộ dữ liệu mô phạm của một hệ thống robot được xây dựng trước.
```sh
wget -qO- https://multiarm.cs.columbia.edu/downloads/data/tasks.tar.xz | tar xvfJ -
wget -qO- https://multiarm.cs.columbia.edu/downloads/data/expert.tar.xz | tar xvfJ -
```
Huấn luyện mô hình chuyển động phân tán cho hệ thống robot với: 
```sh
mkdir runs
python main.py --config configs/default.json --tasks_path tasks/ --expert_waypoints expert/ --num_processes 16 --name multiarm_motion_planner
```
