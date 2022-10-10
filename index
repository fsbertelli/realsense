import depthai as dai
import cv2
import RPi.GPIO as GPIO
import time
import imutils


print("Load all libraries")
GPIO.setwarnings(False)
GPIO.setmode(GPIO.BOARD)
ledPin = 12
buttonPin = 7
GPIO.setup(ledPin, GPIO.OUT)
GPIO.setup(buttonPin, GPIO.IN, pull_up_down=GPIO.PUD_UP)


print("Create camera config")
pipeline = dai.Pipeline()
camRgb = pipeline.createColorCamera()
xoutRgb = pipeline.createXLinkOut()
xoutRgb.setStreamName("rgb")
camRgb.setPreviewSize(1280, 720)
camRgb.preview.link(xoutRgb.input)
camRgb.setFps(60)
device_info = dai.DeviceInfo("172.16.2.100")


print("Starting camera ...")
with dai.Device(pipeline, device_info) as device:
    qRgb = device.getOutputQueue(name="rgb", maxSize=4, blocking=False)
    i = 0
    while True:
        buttonState = GPIO.input(buttonPin)
        frame = qRgb.get().getCvFrame()
        show_frame = imutils.resize(frame, width=640)
       
        if buttonState == False:
            GPIO.output(ledPin, GPIO.HIGH)
            time.sleep(0.8) # depois tu altera
            
            data = time.ctime()
            data = str(data)
            
            name_image = "./images/{}.jpg".format(data)
            
            cv2.imwrite(name_image, frame) 
            print(f"Image: *{name_image}* saved.")
            i += 1
        else:
            GPIO.output(ledPin, GPIO.LOW)

        cv2.imshow("Frame", show_frame)
        
        key = cv2.waitKey(1) & 0xFF
        
        if key == ord('q'):
            break
       
        

        #if key == ord('s'):
        #   cv2.imwrite(f"image_{i}.jpg", frame)
        #    print("Image saved :)")
        #i += 1


