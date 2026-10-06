# Running the 6 DOF Bin Pick and Place Demo

In the `demo/` directory, download the benchmark tasks to the `tasks/` directory
```
wget -qO- https://multiarm.cs.columbia.edu/downloads/data/benchmark.tar.xz | tar xvfJ -
mv benchmark tasks/
```

Chạy mô phỏng cho 500 lần thử nghiệm
```sh
python demo.py
```

Quá trình trên sẽ tạo ra một thư mục kết quả đạt được - nơi lưu những file csv chứa kết quả thực hiện, và một thư mục mô phỏng khác chứa thông số của quá trình mô phỏng.

Đánh giá kết quả:
```sh
python evaluate_results.py --result_dir path/to/results/dir
```
trong thư mục `path/to/results/dir` sẽ chứa file kết quả như: `results_2021-01-01_18-06-35`. Dữ liệu đầu ra có dạng như sua.
```json
{
    "num_exps": 500,
    "num_valid_exps": 298,
    "num_success": 232,
    "avg_steps": 6206.353448275862,
    "num_plane_collision": 31,
    "num_robot_collision": 35,
    "num_timeout": 0,
    "success_rate": 0.7785234899328859
}
```

Để mô hình hóa quá trình mô phỏng, tải xuống và cài đặt[PyBullet Blender Plugin](https://github.com/huy-ha/pybullet-blender-recorder), sau đó import file thông số mô phỏng vào giao diện sử dụng của Blender.
