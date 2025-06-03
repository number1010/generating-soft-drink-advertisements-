# Hệ Thống Tạo Quảng Cáo Nước Giải Khát

Dự án này triển khai một hệ thống học sâu để tạo quảng cáo nước giải khát sử dụng mô hình diffusion. Hệ thống có thể tạo hình ảnh chất lượng cao từ mô tả văn bản hoặc biến đổi hình ảnh hiện có.

## Yêu Cầu

### Môi Trường (Google Colab)
- Tài khoản Google
- Google Drive để lưu trữ dữ liệu và model
- Kết nối internet ổn định
- Các thư viện Python (được cài đặt tự động trong notebook):
  - torch
  - torchvision
  - transformers
  - numpy
  - pillow
  - tqdm
  - pytorch_lightning
  - matplotlib (để hiển thị ảnh)

### Cho Inference (Local)
- Python 3.8+
- PyTorch 1.7+
- GPU tương thích CUDA (khuyến nghị)
- Các gói Python (cài đặt qua `pip install -r requirements.txt`):
  - torch
  - numpy
  - Pillow
  - transformers
  - tqdm

## Cài Đặt

1. Clone repository:
```bash
git clone https://github.com/number1010/generating-soft-drink-advertisements-.git
cd generating-soft-drink-advertisements-
```

2. Upload toàn bộ code lên Google Drive

3. Tải dữ liệu và model:
- Truy cập [Google Drive](https://drive.google.com/drive/folders/11JDKr-BKGEUDLXRfp1y31QyGYi5T55la?usp=sharing)
- Tải các file sau và đặt vào thư mục `data` trong Google Drive:
  - `v1-5-pruned-emaonly.ckpt` (file model đã train)
  - `vocab.json` và `merges.txt` (file tokenizer CLIP)
  - Thư mục `train` và `test` (dataset để training và testing)

## Sử Dụng

### Training và Inference

1. Mở file `sd/train_model.ipynb` trong Google Colab

2. Chạy các cell theo thứ tự:
   - Cell 1: Mount Google Drive để truy cập dữ liệu
   - Cell 2: Cài đặt các thư viện cần thiết
   - Cell 3: Import các thư viện và thiết lập môi trường
   - Cell 4: Định nghĩa dataset và dataloader
   - Cell 5: Thiết lập model, optimizer và bắt đầu training
   - Cell 6: Thử nghiệm model đã train với các prompt từ tập test

3. Các tham số training quan trọng:
   - Batch size: 1
   - Learning rate: 1e-4
   - Số epochs: 3
   - Optimizer: Adam
   - Loss function: MSE Loss
   - Mixed precision training được sử dụng để tối ưu bộ nhớ

4. Sau mỗi epoch, model sẽ được lưu trong thư mục `models` với tên:
   - `encoder_finetuned_epoch_X.pth`
   - `decoder_finetuned_epoch_X.pth`

5. Phần inference đã được tích hợp sẵn trong notebook với các tham số:
   - `cfg_scale`: 25.0 (điều chỉnh mức độ tuân theo prompt)
   - `n_inference_steps`: 50 (số bước khử nhiễu)
   - `seed`: 42 (để tái tạo kết quả)

## Kiến Trúc Mô Hình

Hệ thống sử dụng kết hợp:
- CLIP để mã hóa văn bản
- VAE để mã hóa/giải mã ảnh
- UNet cho quá trình diffusion
- DDPM sampler để tạo ảnh

## Tham Số

- `prompt`: Mô tả văn bản của ảnh mong muốn
- `uncond_prompt`: Prompt phủ định để hướng dẫn quá trình tạo
- `do_cfg`: Bật/tắt classifier-free guidance
- `cfg_scale`: Tỷ lệ guidance (1-14)
- `strength`: Cho biến đổi ảnh, kiểm soát mức độ biến đổi (0-1)
- `n_inference_steps`: Số bước khử nhiễu (mặc định: 50)
- `seed`: Seed ngẫu nhiên để tái tạo kết quả

## Cấu Trúc Thư Mục

```
generating-soft-drink-advertisements/
├── data/
│   ├── train/
│   │   ├── images/     # Ảnh training
│   │   └── prompts.txt # Mô tả tương ứng
│   ├── test/
│   │   ├── images/     # Ảnh test
│   │   └── prompts.txt # Mô tả tương ứng
│   ├── vocab.json      # File tokenizer CLIP
│   ├── merges.txt      # File tokenizer CLIP
│   └── v1-5-pruned-emaonly.ckpt  # Model đã train
├── sd/                 # Source code
├── checkpoints/        # Lưu model khi training
├── requirements.txt    # Dependencies
└── README.md          # Hướng dẫn sử dụng
```

## Kết Luận và Hạn Chế

### Kết Quả Đạt Được
- Mô hình có thể tạo ra các thiết kế nhãn nước giải khát với màu sắc và bố cục phù hợp
- Có khả năng tạo ra các chi tiết phức tạp như gradient, hiệu ứng ánh sáng
- Tạo được các thiết kế theo phong cách cổ điển và hiện đại

### Hạn Chế
1. Thời gian training:
   - Cần nhiều giờ để training mô hình
   - Yêu cầu GPU mạnh để đạt hiệu quả tốt

2. Chất lượng sinh ảnh:
   - Gặp khó khăn trong việc tạo chữ rõ ràng và đọc được
   - Đôi khi tạo ra các chi tiết không mong muốn hoặc biến dạng
   - Cần điều chỉnh nhiều tham số để có kết quả tốt

3. Yêu cầu phần cứng:
   - Cần GPU với VRAM đủ lớn
   - Tốn nhiều tài nguyên tính toán

### Hướng Phát Triển
- Cải thiện khả năng tạo chữ bằng cách thêm các kỹ thuật xử lý hậu kỳ
- Tối ưu hóa thời gian training
- Nghiên cứu thêm các phương pháp để tăng chất lượng ảnh sinh 