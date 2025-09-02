# KMUTT Location Prediction By Image Search
จริงๆก็มีพวก GPS แต่อยากทำเล่นๆเป็น algorithm ไม่ซับซ้อน ไม่ต้องทำโมเดล Image อาจารย์แค่อยากได้โมเดลเชิง algorithm มากกว่าเลยเลือกวิธีนี้ส่งอาจารย์

<br>

### Form Capstone G10 - Bukatsu

<br>

## Abstract

   KMUTT Location Prediction  เป็นหนึ่งใน AI ที่นำมา integrate เข้ากับ Bukatsu Web Application
	โดยที่ concept คือ การที่เราสามารถรู้ได้ว่า ภาพที่เราถ่าย คือจุดไหนในมหาลัย และสามารถใช้ map ต่อได้หลังจากที่รู้จุดแล้ว

## Algorithm 
ใช้ *SIFT + FLANN* เป็นหลักซึ่งแค่นี้ก็เพียงพอต่อการทำ Image Search แล้ว <br>
*SIFT (Scale-Invariant Feature Transform)* เป็น **feature extraction algorithm** ใช้สำหรับหา **keypoints** (จุดสำคัญในภาพ เช่น มุม, จุดเด่น) และสร้าง **descriptor** เพื่อบรรยายลักษณะเฉพาะของจุดนั้น ๆ <br>

*FLANN (Fast Library for Approximate Nearest Neighbors)*  เป็น **library** ที่ช่วยค้นหา "neighbors" (ใกล้ที่สุด) ในชุดข้อมูลที่มีมิติสูงได้ (high-dimensional data) <br> <br>
*source* : <br>
[SIFT](https://docs.opencv.org/4.x/da/df5/tutorial_py_sift_intro.html)  <br>
[FLANN](https://docs.opencv.org/3.4/d5/d6f/tutorial_feature_flann_matcher.html) <br>
[Paper](https://arxiv.org/html/2407.14910v1)

## Dataset Collection

ถ่ายเป็น Panorama รอบมหาลัย พร้อม Label พิกัด lat long <br>
```
dataset/
├── zone_1/
├── zone_2/
├── zone_3/
│   ├── zone_3_1_1/
│   │   ├── coords.json
│   │   └── IMG_8745.jpg
│   ├── zone_3_2_1/
│   ├── zone_3_3_1/
│   ├── zone_3_4_1/
│   ├── zone_3_5_1/
│   └── zone_3_6_1/
├── zone_4/
├── zone_5/
└── zone_6/

```
ใน coords.json
```
{
  "lat": 13.649275908884414,
  "lon": 100.49315745514194
}

```
โดยแบ่ง zone เพื่อให้ group ของบริเวณมหาลัยให้จัดการ Data ได้ง่าย เพิ่มข้อมูลต่อไปได้ง่าย ไม่มีผลต่อ Accuracy 

## Preprocess Data

หลักๆจะใช้ data จริงๆที่ไม่ได้ปรับสีหรืออะไรเลย แต่จะเป็นการลดความละเอียดของภาพ เพื่อประหยัดเวลาและ memory แทน ซึ่งถ้าเกิดว่าคอมแรงจริงๆ สามารถทำให้ภาพชัดตามความเหมาะสมได้เลย

![Preprocess Image](https://github.com/wachawich/KMUTT-Location-Prediction/blob/main/image/preprocess_image.png)

## Pipeline

![Pipeline Image](https://github.com/wachawich/KMUTT-Location-Prediction/blob/main/image/pipeline_image.png)

## Sample Result

ฝั่งซ้ายคือรูปจากมือถือ ฝั่งขวาคือรูปที่ match ได้ เอามาเทียบกันให้ดู <br>

### True Result

![Result Image](https://github.com/wachawich/KMUTT-Location-Prediction/blob/main/image/result_1.png)
![Result Image](https://github.com/wachawich/KMUTT-Location-Prediction/blob/main/image/result_2.png)
![Result Image](https://github.com/wachawich/KMUTT-Location-Prediction/blob/main/image/result_3.png)
![Result Image](https://github.com/wachawich/KMUTT-Location-Prediction/blob/main/image/result_4.png)

### False Result

![Result Image](https://github.com/wachawich/KMUTT-Location-Prediction/blob/main/image/result_5.png)

## Conclusion
สรุปก็คือ การทำ Location Prediction ด้วยวิธีนี้ แน่นอนว่าไม่ค่อยซับซ้อน ง่าย ใช้ CPU รันก็ได้ ใช้แค่ algorithm แบบไม่ลงลึกที่อาจารย์ต้องการ ไม่ต้องถึงขั้นเป็น Image Model แต่ก็ต้องแลกกับ การกิน memory และใช้เวลาในการ search O(n) และจำเป็นต้องใช้ Multi Core , Parallel Processing ถึงจะได้ประสิทธิภาพที่สุด แต่ฮาร์ดแวร์ก็จะทำงานหนักไปด้วย

<br>
<br>
<br>

*Develop by Wachirawit Premthaisong*
