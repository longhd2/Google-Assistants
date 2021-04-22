

### Fix, mod Google-Assistants tiếng việt từ trang 
```sh
https://github.com/shivasiddharth/Assistants-Pi
```
là dự án  miễn phí, phục vụ cá nhân.
### 1.Tải OS tại đây và đăng ký credentials :
```sh
http://www.cs.tohoku-gakuin.ac.jp/pub/Linux/RaspBerryPi/
```
Tải credentials--->.json (https://developers.google.com/assistant/sdk/develop/python/config-dev-project-and-account)   

### 2.Update OS & cài đặt git:
```sh
sudo apt-get update && sudo apt-get install git -y
```
### 3.Cài đặt Mic & Loa nếu sử dụng Mic HAT:
```sh
cd /home/${USER}/
git clone https://github.com/respeaker/seeed-voicecard.git
cd seeed-voicecard
sudo ./install.sh --compat-kernel
sudo reboot
speaker-test
```
Sau khi khởi động lại, đăng nhập lại vào console
Thống kê ID của Mic USB và Loa
```sh
arecord -l
aplay -l
```
==> Xong chuyển sang bước 5.
### 4.Cài đặt Mic & Loa nếu sử dụng Mic USB.
Chạy lệnh sau
```sh
sudo nano /home/pi/.asoundrc
```
Cửa sổ nano hiện lên, paste dòng sau, thay thế ID mic, loa phù hợp

```sh
pcm.dsnooper {
    type dsnoop
    ipc_key 816357492
    ipc_key_add_uid 0
    ipc_perm 0666
    slave {
        pcm "hw:1,0"
        channels 1
    }
}

pcm.!default {
        type asym
        playback.pcm {
                type plug
                slave.pcm "hw:0,0"
        }
        capture.pcm {
                type plug
                slave.pcm "dsnooper"
        }
}

```
Coppy cấu hình âm thanh vào etc:
```sh
sudo cp /home/pi/.asoundrc /etc/asound.conf
```
==> Chạy lệnh sau để đưa Account đang dùng  vào group root
```sh
sudo usermod -aG root pi
```
### 5.Tạo file audiosetup (đặt tên mic tương ứng trong file) trong thư mục /home/pi
```sh
USB-MIC-JACK
USB-MIC-HDMI
AIY-HAT
Respeaker-2-Mic
Respeaker-4-Mic
Respeaker-Usb-Mic
```
### 6.Tải git & cài đặt:
```sh
git clone https://github.com/longhd2/Google-Assistants
```
Chạy lệnh sau để cài đặt:
```sh
 sudo chmod +x ./Google-Assistants/install/gassist-installer.sh && sudo  ./Google-Assistants/install/gassist-installer.sh
```
### 7.Chạy lần đầu:
```sh
env/bin/python -u ./Google-Assistants/src/main.py --project-id 'XXX' --device-model-id 'XXX'
```
### 8.Chạy thủ công các lần tiếp theo:
```sh
env/bin/python -u ./Google-Assistants/src/main.py
```
### Fix pluseaudio
```sh
sudo apt-get update     
sudo apt-get install git    
cd /home/${USER}/       
git clone https://github.com/shivasiddharth/PulseAudio-System-Wide       
cd ./PulseAudio-System-Wide/      
sudo cp ./pulseaudio.service /etc/systemd/system/pulseaudio.service    
systemctl --system enable pulseaudio.service       
systemctl --system start pulseaudio.service       
sudo cp ./client.conf /etc/pulse/client.conf        
sudo sed -i '/^pulse-access:/ s/$/root,pi/' /etc/group    
```



