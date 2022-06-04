# implement_RTP

1 Lý thuyết
1.1 RTSP là gì ?
Được phát triển bởi RealNetworks, Netscape và Đại học Columbia, với bản thảo đã được gửi lên IETF kể từ năm 1996, RTSP (Real Time Streaming Protocol - Giao thức truyền tin thời gian thực) là một giao thức điều khiển phương tiện mạng ở tầng ứng dụng (Application Layer) được thiết kế để sử dụng trong các hệ thống giải trí và truyền thông để điều khiển máy chủ chứa các dữ liệu truyền tin đa phương tiện (streaming media). Giao thức này được sử dụng để   thiết lập và điều khiển các phiên truyền thông giữa các endpoint. Các client của server phát ra các lệnh kiểu VHS, như chơi (play), ghi hình/âm (record) và dừng (pause), để tạo yêu cầu điều khiển thời gian thực các gói tin phương tiện từ server đến client (Video On Demand) hoặc từ client đến server (Voice Recording).
Việc truyền tải dữ liệu trực tuyến không phải là một nhiệm vụ của RTSP. Hầu hết các máy chủ RTSP sử dụng giao thức truyền tải thời gian thực (RTP - Real-time Transport Protocol) kết hợp với giao thức điều khiển thời gian thực (RTCP - Real-time Control Protocol) để phân phối luồng phương tiện. Tuy nhiên, một số nhà cung cấp sử dụng các giao thức truyền tải độc quyền. Ví dụ: Phần mềm máy chủ RTSP của RealNetwork, cũng sử dụng Real Data Transport (RDT) độc quyền của RealNetworks.
RTSP được chuẩn hóa bởi MMUSIC WG (Multiparty Multimedia Session Control Working Group) của IETF (Internet Engineering Task Force) và được công bố với tên gọi RFC 2326 vào năm 1998. RTSP 2.0 được công bố với tên gọi RFC 7826 vào năm 2017 với vai trò thay thế cho RTSP 1.0. RTSP 2.0 dựa trên nền tảng RTSP 1.0 nhưng không có tính tương thích ngược với các cơ chế ngoài cơ chế đàm phán trong phiên bản cơ bản.
Trong khi có một vài tính chất giống với giao thức HTTP (HyperText Transfer Protocol - Giao thức truyền tải siêu văn bản), RTSP tập trung định nghĩa các chuỗi điều khiển cho việc điều khiển các luồng phát lại thông tin truyền thông đa phương tiện. RTSP có các trạng thái (state), một loại danh hiệu dùng để truy vết các phiên làm việc đồng thời, trong khi HTTP là stateless. RTSP sử dụng TCP (Transmission Control Protocol - Giao thức điều khiển truyền vận) để duy trì một kết nối end-to-end. Mặc dù phần lớn các thông điệp RTSP được gửi từ phía client đến server, vẫn có các thông điệp truyền theo chiều ngược lại, từ server đến client.
1.2 RTP Socket là gì ?
Được phát triển bởi AVTWG (Audio-Video Transport Working Group) của IETF và được công bố lần đầu với tên gọi RFC 1889 vào năm 1996 và sau đó thay thế bởi RFC 3550 vào năm 2003, RTP (Real-time Transport Protocol - Giao thức vận chuyển thời gian thực) là một giao thức mạng máy tính được dùng cho truyền tải âm thanh và video thông qua mạng Internet. RTP được sử dụng trong các hệ thống kết nối và giải trí có liên quan đến đường truyền truyền thông. Thông thường RTP hoạt động thông qua UDP, được sử dụng cùng với RTCP (RTP Control Protocol - Giao thức điều khiển RTP). Trong khi RTP giữ đường truyền truyền thông, RTCP được sử dụng để giám sát các thông số truyền tải và chất lượng của dịch vụ và hỗ trợ vấn đề đồng bộ của nhiều luồng đường truyền. RTP là một trong những nền tảng kỹ thuật của Voice over IP.
1.3 WebSocket là gì ?
Giao thức Websocket cho phép giao tiếp 2 chiều giữa một client chạy các đoạn code không được xác thực trong một môi trường được kiểm sát và một Remote host mà đã lựa chọn kết nối đến đoạn code đó. Mô hình bảo mật được sử dụng dựa trên mô hình gốc thường được các trình duyệt web sử dụng.
Giao thức bao gồm một bắt đầu mở đầu nối tiếp theo bởi khung tin nhắn cơ bản, The protocol consists of an opening handshake, bao bọc bởi TCP ở lớp thứ 4, mặc dù hoạt động ở tầng ứng dụng của mô hình OSI. Mục tiêu của công nghệ này là cung cấp một phương thức để các ứng dụng dựa trên nền tảng trình duyệt cần đến giao tiếp 2 chiều với Server không dựa vào việc mở nhiều kết nối HTTP.
Giao thức Websocket được tiêu chuẩn hóa bởi IETF với tên gọi RFC 6455 vào năm 2011, và Websocket API trong Web IDL (một ngôn ngữ mô tả giao diện - interface description language (IDL) được dùng để định nghĩa các API được sử dụng trong trình duyệt web) đang được chuẩn hóa bởi W3C.
Hiện nay, hầu hết tất cả trình duyệt web phổ biến như Google Chrome, Microsoft Edge, Internet Explorer, Firefox, Safari, . . . đều hỗ trợ giao thức Websocket
2 Phân tích yêu cầu
2.1 Yêu cầu chức năng
•   Triển khai một mô hình giao tiếp server-client được hiện thực bằng giao thức RTSP và gửi dữ liệu bằng giao thức RTP.
•   Client khởi động sẽ mở RTSP socket đến server để gửi yêu cầu cho server.
•   Trạng thái của Client liên tục cập nhật khi nhận phản hồi từ server.
2.2 Yêu cầu phi chức năng
•   Thời gian chờ của datagram socket để nhận RTP data từ server là 0.5s.
•   Server gửi gói RTP cho client bằng UDP mỗi 50ms.
3 Hiện thực
3.1 Client side
Một giao diện được tạo ra từ file ClientLauncher.py khi chạy chương trình với các nút bấm SETUP, PLAY, PAUSE, TEARDOWN để người dùng có thể thực hiện các thao tác với video. Tương ứng với mỗi nút bấm, video sẽ có từng trạng thái khác nhau lần lượt là INIT, READY, PLAYING, mối quan hệ với các nút bấm được thể hiện như hình dưới

 
Việc hiện thực RTSP ở client side được thao tác chủ yếu ở hàm def sendRtspRequest(seft, requestCode) được đặt trong file Client.py.
Các request luôn thực hiện thao tác chính là: tăng sequence lên 1, ghi request dưới dạng chuỗi, cập nhật lại trạng thái.
Các nút SETUP, PLAY, PAUSE, TEARDOWN khi được bấm sẽ gọi lần lượt theo 4 hàm, cụ thể:
•   Nút SETUP gọi hàm setupMovie
    def setupMovie(self):
        """Setup button handler."""
        if self.state == self.INIT:
            self.sendRtspRequest(self.SETUP)    

•   Nút PLAY gọi hàm playMovie
def playMovie(self):
        """Play button handler."""
        if self.state == self.READY:
            # Create a new thread to connect to server and listen to the change on server
            threading.Thread(target=self.listenRtp).start()
            # Create a variable to save the next event after click on the button "Play"
            self.playEvent = threading.Event()
            # Block thread until the request PLAY send to server and client receive the response
            self.playEvent.clear()
            # Send request to server
            self.sendRtspRequest(self.PLAY)

•   Nút PAUSE gọi hàm pauseMovie
    def pauseMovie(self):
        """Pause button handler."""
        if self.state == self.PLAYING:
            self.sendRtspRequest(self.PAUSE)

•   Nút  TEARDOWN  gọi  hàm  exitClient
    def exitClient(self):
        """Teardown button handler."""
        self.sendRtspRequest(self.TEARDOWN)    
        self.master.destroy() # Close the gui window
        os.remove(CACHE_FILE_NAME + str(self.sessionId) + CACHE_FILE_EXT) # Delete the cache image from video
Có 4 request chính được thể hiện thông qua 4 hàm trên, bao gồm:
•   SETUP: khi request này được gửi đi, một thread sẽ được tạo ra trên client để nhận phản hồi từ server. Hàm recvRtspReply sẽ luôn chờ giải mã request từ server (một vòng lặp while True).
Trong quá trình tiến hành giải mã phản hồi tại hàm parseRtspReply, client sẽ cập nhật trạng thái tùy vào hành động mà nó nhận được. Đối với tín hiệu là SETUP thì client sẽ tạo ra một socket để nhận data với thời gian chờ là 0.5s. Câu lệnh tạo socket được hiện thực ở hàm openRtpPort.
Hàm openRtpPort:
        self.sendRtspRequest(self.PAUSE)
        self.rtpSocket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
        # Set the timeout value of the socket to 0.5sec
        self.rtpSocket.settimeout(0.5)  
        try:
            # Bind the socket to the address using the RTP port given by the client user
            self.rtpSocket.bind(("", self.rtpPort))
        except:
            tkinter.messagebox.showwarning('Unable to Bind', 'Unable to bind PORT=%d' %self.rtpPort)
Hàm parseRtspReply:
 
    def parseRtspReply(self, data):
        """Parse the RTSP reply from the server."""
        lines = data.split(b'\n')
        seqNum = int(lines[1].split(b' ')[1])
       
        # Process only if the server reply's sequence number is the same as the request's
        if seqNum == self.rtspSeq:
            session = int(lines[2].split(b' ')[1])
            # New RTSP session ID
            if self.sessionId == 0:
                self.sessionId = session 
            # Process only if the session ID is the same
            if self.sessionId == session:
                if int(lines[0].split(b' ')[1]) == 200: # The status code 200 is OK
                    if self.requestSent == self.SETUP:
                        # Update RTSP state.
                        self.state = self.READY
                        # Open RTP port.
                        self.openRtpPort()
                    elif self.requestSent == self.PLAY:
                        self.state = self.PLAYING
                    elif self.requestSent == self.PAUSE:
                        self.state = self.READY
                        # The play thread exits. A new thread is created on resume.
                        self.playEvent.set()
                    elif self.requestSent == self.TEARDOWN:
                        self.state = self.INIT
                        # Flag the teardownAcked to close the socket.
                        self.teardownAcked = 1

Phần mã thực hiện thao tác gửi đi tín hiệu SETUP trong hàm sendRtspRequest:
       if requestCode == self.SETUP and self.state == self.INIT:
            threading.Thread(target=self.recvRtspReply).start()
            # update rptspSeq = self.[action]
            self.rtspSeq += 1
            # save the content of action
            msg = 'SETUP ' + self.fileName + ' RTSP/1.0\nCSeq: ' + str(self.rtspSeq) + '\nTransport: RTP/UDP; client_port= ' + str(self.rtpPort)
            # keep track the request sent to server
            self.requestSent = self.SETUP
            self.state = self.READY
•   PLAY: một thread sẽ được tạo ra để lắng nghe sự thay đổi của frame và tiến hành cập nhật video đang được phát (hàm listenRtp). Video được cập nhật thay đổi ở hàm writeFrame và hàm updateMovie
Hàm listenRtp:
    def listenRtp(self):        
        """Listen for RTP packets."""
        while True:
            try:
                data, addr = self.rtpSocket.recvfrom(20480) # load all bytes need to display
                if data:
                    rptData = RtpPacket()
                    rptData.decode(data)
                    seqNum = rptData.seqNum()               
                    if seqNum > self.frameNbr: # Discard the late packet
                        self.frameNbr = seqNum
                        self.updateMovie(self.writeFrame(rptData.getPayload())) # send cache name to update movie to change content
            except:
                if self.playEvent.isSet():
                    break            
                if self.teardownAcked == 1:
                    self.rtpSocket.shutdown(socket.SHUT_RDWR)
                    self.rtpSocket.close()
                    break
Hàm writeFrame:
    def writeFrame(self, data):
        """Write the received frame to a temp image file. Return the image file."""
        cachename = CACHE_FILE_NAME + str(self.sessionId) + CACHE_FILE_EXT # name of cache file
        file = open(cachename, "wb") # open file with authorization: write and the standard file is binary
        file.write(data)
        file.close()    
        return cachename
Hàm updateMovie :
    def updateMovie(self, imageFile):
        """Update the image file as video frame in the GUI."""
        photo = ImageTk.PhotoImage(Image.open(imageFile)) # read the data and transger to variable "photo" by using Tk package
        self.label.configure(image = photo, height=288)
        self.label.image = photo # update screen
Phần mã thực hiện thao tác gửi đi tín hiệu PLAYtrong hàm sendRtspRequest:
        elif requestCode == self.PLAY and self.state == self.READY:
            self.rtspSeq += 1
            msg = 'PLAY ' + self.fileName + ' RTSP/1.0\nCSeq: ' + str(self.rtspSeq) + '\nSession: ' + str(self.sessionId)
            self.requestSent = self.PLAY
            self.state = self.PLAYING
•   PAUSE: request sẽ gửi đi tín hiệu dừng, thread plaing được tạo khi request PLAY được gửi sẽ bị xóa (được hiện thực trong hàm parseRtspReply
        elif requestCode == self.PAUSE and self.state == self.PLAYING:
            self.rtspSeq
            msg = 'PAUSE ' + self.fileName + ' RTSP/1.0\nCSeq: ' + str(self.rtspSeq) + '\nSession: ' + str(self.sessionId)
            self.requestSent = self.PAUSE
            self.state = self.READY
•   TEARDOWN: request hủy sẽ được gửi đi, thread kết nối giữa client và server sẽ bị hủy bỏ.
        elif requestCode == self.TEARDOWN and self.state != self.INIT:
            self.rtspSeq += 1
 
            msg = 'TEARDOWN ' + self.fileName + ' RTSP/1.0\nCSeq: ' + str(self.rtspSeq) + '\nSession: ' + str(self.sessionId)
            self.requestSent = self.TEARDOWN
            self.state = self.INIT
        else:
            return
Cuối cùng, thông qua websocket, request được mã hóa và gửi đi tại hàm sendRtspRequest
        # Send request to server using rtspSocket
        self.rtspSocket.sendall(bytes(msg, 'utf8'))

3.2 Server side
Server được khởi tạo từ lời gọi đến class Server và được hiện thực trong class ServerWorker. Khi server chạy, một thread mới được tạo ra để nhận RTSP request từ client. Hàm recvRt- spRequest có nhiệm vụ nhận tín hiệu sau đó chuyển đến hàm processRtspRequest để xử lý request đã được giải mã. Tương tự như ở client, server cũng có các trạng thái là INIT, READY, PLAYING và các hành động SETUP, PLAY, PAUSE, TEARDOWN.
•   Nếu request là SETUP, server sẽ được chuyển sang trạng thái READY, nếu tín hiệu nhận không được thì kết quả trả về là mã 404. Mọi thứ được diễn ra thành công, hàm trả về mã 200.
•   Nếu request là PLAY, server chuyển sang trạng thái PLAYING, socket mới được tạo ra, một thread mới cũng được tạo ra để gửi đi các gói RTP.
•   Nếu request là PAUSE, server sẽ chuyển trạng thái sang READY và thread gửi đi các gói RTP sẽ bị hủy.
•   Nếu request là TEARDOWN, server đóng socket, hủy thread kết nối client.
Trong class ServerWorker, hàm sendRtp sẽ gửi dữ liệu cho client, dữ liệu này sẽ được đóng gói trong rtpPacket trong hàm makeRtp. Hàm encode được hiện thực để mã hóa lại dữ liệu trước khi gửi đi.
Hàm encode:
    def encode(self, version, padding, extension, cc, seqnum, marker, pt, ssrc, payload):
        """Encode the RTP packet with header fields and payload."""
        timestamp = int(time())
        header = bytearray(HEADER_SIZE)
        header[0] = 0x00
        header[0] |= (version & 0b11) << 6
        header[0] |= (padding & 0b1) << 5
        header[0] |= (extension & 0b1) << 4
        header[0] |= (cc & 0b1111)
 
        header[1] = 0x00
        header[1] |= (marker & 0b1) << 7
        header[1] |= (pt & 0b1111111)
 
        header[2] |= (seqnum & 0xFFFF) >> 8
        header[3] |= (seqnum & 0xFF)
 
        header[4] |= (timestamp & 0xFFFFFFFF) >> (32 - 8 * 1)
        header[5] |= (timestamp & 0x00FFFFFF) >> (32 - 8 * 2)
        header[6] |= (timestamp & 0x0000FFFF) >> (32 - 8 * 3)
        header[7] |= (timestamp & 0x000000FF) >> (32 - 8 * 4)
 
        header[8]  |= (ssrc & 0xFFFFFFFF) >> (32 - 8 * 1)
        header[9]  |= (ssrc & 0x00FFFFFF) >> (32 - 8 * 2)
        header[10] |= (ssrc & 0x0000FFFF) >> (32 - 8 * 3)
        header[11] |= (ssrc & 0x000000FF) >> (32 - 8 * 4)
        self.header = header
        # Get the payload from the argument
        # self.payload = ...
        self.payload = payload
 
4 Class diagram 

 

5 Mô tả các tính năng
•   Class Video stream:
–	  	init (self,filename): Mở video có tên là filename, đặt giá trị frameNum ban đầu là 0.
–	nextFrame(self): lấy dữ liệu của frame tiếp theo, cập nhật giá trị frameNum lên 1 đơn vị.
–    frameNbr(self): Lấy giá trị frameNum.
•   Class ServerWorker:
–      	init  	(self): Nhận thông tin của client.
–    run(self): Tạo 1 thread để gọi hàm recvRtspRequest nhận RTSP request.
–	recvRtspRequest(self): Nhận RTSP request, giải mã request và gọi processRtspRe- quest để xử lý tín hiệu.
–	– processRtspRequest(self, data): Nhận tín hiệu đã giải mã, thực hiện công việc khác nhau cho mỗi loại request: PLAY, SETUP, TEARDOWN.
–    sendRtp(self): gửi RTP packets thông qua UDP.
–    makeRtp(self, payload, frameNbr): tạo RTP packet cho video dữ liệu.
–	replyRtsp(self, code, seg): Trả lời tín hiệu RTSP về client. Tín hiệu có thể Sucess- ful(200_OK) hoặc "404 NOT FOUND" hoặc "500 CONNECTION ERROR".
•   Class Rtpacket:
–          init	 (self): khởi tạo đối tượng thuộc lớp RtpPacket.
–	encode(self,version, padding, extension, cc, seqnum, market, pt, ssrc, payload): gán các giá trị của extension, cc, seqnum, market, pt, ssrc vào header field của gói tin RTP và gán payload vào payload field của gói tin RTP.
–	decode (self, byteStream): từ gói tin RTP, hàm sẽ tách ra thành 2 phần header và payload.
–    version(self): trả về giá trị của RTP version trong header field.
–    seqNum(self): trả về giá trị của sequence (frame) number trong header field.
–    timestamp(self): trả về giá trị của timestamp trong header field.
–    payloadType(self): trả về giá trị của payload type trong header field.
–    getPayload(self): trả về dữ liệu của payload.
–    getPacket(self): trả về gói tin RTP bao gồm header và payload.
•   Class Server:
–    main(self): Mở socket, chạy ServerWorker.
•   Class Client:
–	      init (self, master, serveraddr, serverport, rtpport, filename): Nhận các giá trị vào, gọi hàm createWidgets() để tạo các button và các label, gọi connectToServer để kết nối đến server.
–    createWidgets(self): tạo giao diện trên client.
–    setupMovie(self): Hàm xử lý button SETUP.
–    exitClient(self): Hàm xử lý button TEARDOWN.
–    pauseMove(self): Hàm xử lý button PAUSE.
–    playMovie(self): Hàm xử lý button PLAY.
–    listenRtp(self): Hàm nhận tín hiệu RTP packets.
–    writeFrame(self, data): Ghi khung hình vào một file tạm thời, trả về file tạm thời đó.
–    connectToServer(self): Kết nối đến server, bắt đầu một RTSP/TCP session mới.
–    sendRtspRequest(self, requestCode): Gửi tín hiệu RTSP đến server.
–    recvRtspReply(self): Nhận tín hiệu RTSP từ server.
–	parseRtspReply(self, data): Xử lý tín hiệu đã được giải mã, ứng với từng tín hiệu khác nhau sẽ có cách xử lý khác nhau.
–    openRtpPort(self): Mở cổng RTP.
–    handler(self): Xử lý khi đóng cửa sổ client.
6 Phần mở rộng: 
6.1  Số liệu thống kê của session 
Các giá trị cần tính trong yêu cầu này là: tổng số bytes dữ liệu của video server đã truyền xuống client, tốc độ truyền dữ liệu video, và tỉ lệ RTP packet loss rate. Các giá trị thông tin sẽ được lưu trong các biến:
•   timePeriod: tổng lượng thời gian từ đầu cho đến kết thúc video.
•   totalData: tổng data nhận được từ server.
•   totalFrame: tổng số frame nhận được từ server.
•   loss: số lượng frame bị mất.
•   startRecv: lưu giá trị thời gian khi bắt đầu gửi đi request để nhận 1 packet từ server.
•   endRecv: lưu giá trị thời gian khi kết thúc sau khi nhận được 1 packet từ server.
• speed: lưu tốc độ hiện tại của video (frames per second)
Để màn hình hiển thị ra thông tin mong muốn, cần xây dựng thêm cửa sổ hiển thị thông tin ở class Client.py.
Hàm createWidget:
def createWidget(self)
...
# Create Text
self.ann  =  Text(self.master,  width=40,  padx=3,  pady=3,  height=10) self.ann.grid(row=3,  columnspan=2)
…
* Hiện thực lấy các giá trị video loss rate, video data rate và video data received được đặt trong hàm listenRtp và playMovie Hàm playMovie:
def playMovie(self):
...
self.startRecv  =  time.time()
....
Hàm listenRtp:
def listenRtp(self):
...
if  seqNum  >  self.frameNbr  +  1  and  self.tempFrameNum  !=  -1: self.loss += seqNum - self.frameNbr
self.timePeriod += self.endRecv - self.startRecv self.totalData += len(rtpData.getPayload()) self.startRecv = time.time()
…
Ở mỗi lần đọc RTP packets, nếu như ta nhận được một packet mới, tổng thời gian chạy video sẽ được cập nhật bằng cách lấy thời gian hiện tại trừ cho thời gian ta đã nhận từ packet lần trước hoặc nếu đó là packet đầu tiên sau khi bấm nút Play thì sẽ trừ cho thời gian khi bấm nút Play. Các giá trị sẽ được tính như sau:
•    video data received: là giá trị của biến totalData.
•  packet lost rate: loss / totalFrame
•  video data rate: totalData / timePeriod.

	* Hiện thực giá trị FPS:
•	Sau mỗi lần gọi hàm faster thìì speed sẽ tăng 2 lần 
 
•	Sau mỗi lần gọi hàm lower thì speed sẽ giảm 2 lần
 
 	FPS = speed 
Kết quả thu được sau khi chạy:
 
Như hình trên, ta thấy được kết quả có 188729 bytes dữ liệu nhận được ,RTP packet loss rate là: 0.0%, video data rate: 103364.53511181798 (bytes/s), FPS hiện tại là 20.
6.2  Chế độ 3 nút bấm Play, Pause, Stop
6.2.1 Loại bỏ nút Setup
Để loại bỏ nút setup thì ta sẽ sửa lại GUI thông qua hàm createWidgets và tích hợp setup vào nút play. Lần nhấn nút Play đầu tiên ta sẽ gửi tính hiệu setup đến server và sau khi nhận được reply từ server thì ta sẽ gọi tiếp hàm playMovie và mở RTP port để chạy video. Trong các lần nhấn nút Play tiếp theo thì ta chỉ gọi hàm playMovie mà không cần setup nữa.
Cụ thể đối với hàm playMovie:
 
Hàm parseRtspReply
	def parseRtspReply(self, data):
        """Parse the RTSP reply from the server."""
        			. . .
            if self.sessionId == session:
                if int(lines[0].split(b' ')[1]) == 200: # The status code 200 is OK
                    if self.requestSent == self.SETUP:
                        # Update RTSP state.
                        self.state = self.READY
                        # Open RTP port.
                        self.openRtpPort()
                        self.playMovie()
Với SETUP request được tích hợp sẵn trong nút Play, người dùng sẽ dễ dàng hơn trong việc sử dụng ứng dụng. Ở button Stop, hoàn toàn thích hợp để gửi TEARDOWN khi người dùng gửi nhấn vào nút Stop. Bởi vì khi người dùng bấm Stop, tức có nghĩa người dùng muốn dừng hẳnviệc truyền video, đó chính là chức năng của TEARDOWN request.
Hình dưới đây là giao diện của 3 nút bấm:
 
6.2.2 Also, is it appropriate to send TEARDOWN when the user clicks on the STOP button?
- Theo em việc gửi teardown (hay ngắt kết nối) là hợp lý khi nhấn nút Stop bởi vì máy chủ phải duy trì session cho một client cụ thể, máy chủ không biết các lệnh được gửi cùng một đường TCP nhưng máy chủ sẽ cung cấp một sessionID được tạo ra khi phản hồi SETUP đầu tiên trả về client và sau đó các lệnh sẽ được gửi kèm với sessionID đó. Bằng cách gửi yêu cầu TEARDOWN, client có thể cho máy chủ biết rằng nó có thể giải phóng tất cả trạng thái liên quan đến người dùng đó.
- PAUSE sẽ chỉ hướng dẫn máy chủ ngừng gửi các gói chứ không phải đóng trạng thái liên quan đến session đó
6.3  Hiện thực DESCRIBE request
Khi người dùng ấn vào button Describe, một request DESCRIBE sẽ được gửi lên server, và sau đó server sẽ trả lại một session mang thông tin của file được xác định bởi tên file (filename) mà client gửi đến cho server, sẽ giúp cho client biết đang truyền dữ liệu như thế nào.
Giao thức được server dùng để gửi phản hồi cho client trong trường hợp mặc định là SDP (Session Description Protocol - Giao thức mô tả phiên làm việc), là một giao thức để mô tả các thông số khởi tạo dòng thông tin phương tiện (streaming media)
Để hiện thực tính năng trên, cần phải thêm một biến DESCRIBE cho class Client đồng thời tại class ServerWorker phải có hàm đặc biệt riêng để xử lý request DESCRIBE được gửi lên từ client và trả về cho phù hợp với định dạng SDP.
Class Client:
-	Hàm describeMovie: để xử lý thao tác bấm vào nút DESCRIBE:
    def describeMovie(self):
        """Describe button handler."""
        self.sendRtspRequest(self.DESCRIBE)
-	Hàm sendRtspRequest: gửi request lên server
 
    def sendRtspRequest(self, requestCode):
		. . .
        elif requestCode == self.DESCRIBE and (self.state == self.PLAYING or self.state == self.READY):
            self.rtspSeq += 1
            msg = 'DESCRIBE ' + self.fileName + ' RTSP/1.0\nCSeq: ' + str(self.rtspSeq) + '\nSession: ' + str(self.sessionId)
            self.requestSent = self.DESCRIBE
-	  Hàm parseRtpReply: để giải mã tín hiệu gửi về từ server và hiển thị ra giao diện:
 
    def parseRtspReply(self, data):
		. . .
                    elif self.requestSent == self.DESCRIBE:
                        temp = lines[3].decode()
                        for i in range(4, len(lines)):
                            temp += '\n' + lines[i].decode()
                        self.des.insert(INSERT, temp + '\n\n')
Class ServerWorker:
•   Hàm processRtspRequest:
    def processRtspRequest(self, data):
		. . .
        elif requestType == self.DESCRIBE:
            print("processcing DESCRIBE\n")
            self.replyDes(self.OK_200, seq[1], filename)
•   Hàm replyDes: gửi trả đoạn mã thông tin về cho client:
    def replyDes(self, code, seq, filename):
        body  = "\n\nv=0"
        body += "\nm=video " + str(self.clientInfo['rtpPort']) + " RTP/AVP " + "26"
        body += "\na=control:streamid=" + str(self.clientInfo["session"])
        body += "\na=\mimetype:string;\"video/MJPEG\""
        if code == self.OK_200:
            reply  = 'RTSP/1.0 200 OK\nCSeq: ' + seq + '\nSession: ' + str(self.clientInfo['session'])
            reply += '\nContent−Base: ' + filename
            reply += '\nContent−Type: application/sdp'
            reply += '\nContent−Length: ' + str(len(body)) + body
           
            self.clientInfo["rtspSocket"][0].send(reply.encode())
        elif code == self.FILE_NOT_FOUND_404:
            print("404 NOT FOUND")
        elif code == self.CON_ERR_500:
            print("500 CONNECTION ERROR")
Các thông số server phản hồi cho client gồm có:
•   v: phiên bản của giao thức SDP được sử dụng.
•   m: tên phương tiện truyền thông và địa chỉ transport.
•   a: không có hoặc là các dòng mô tả thêm các thuộc tính của phương tiện truyền thông
•   mime type: nhãn dùng để nhận biết kiểu của dữ liệu.
 
6.4  Chế độ chọn nhiều video
Tại chức năng chọn video, nhóm đề xuất ý tưởng thêm một trạng thái mới cho client là SWITCH. Ngay khi client được chạy trạng thái của client sẽ được chuyển từ INIT sang SWITCH, khi đó một request sẽ được gửi đến server để lấy về danh sách các video đang có trên server và hiển thị về cho client. Sau đó, người dùng thực hiện thao tác chọn video và bấm PLAY để phát video như bình thường. 
Class Client:
•   Hàm sendRtspRequest: gửi đi request ngay khi trình phát video được mở.
    def sendRtspRequest(self, requestCode):
		. . .
        if requestCode == self.LOAD and self.state == self.INIT:
            threading.Thread(target=self.recvRtspReply).start()
            self.rtspSeq += 1
            msg = 'LOAD ' + self.fileName + ' RTSP/1.0\nCSeq: ' + str(self.rtspSeq) + '\nSession: ' + str(self.sessionId)
            self.requestSent = self.LOAD
•   Hàm parseRtspReply: giải mã đoạn dữ liệu tương ứng là danh sách các video được tải về từ server.
 
    def parseRtspReply(self, data):
		. . .
                    if self.requestSent == self.LOAD:
                        temp = lines[3].decode()[8: ].split(',')
                        self.videos = temp
                        self.setList()
                        self.state = self.SWITCH
 •   Hàm setList: duyệt qua các phần tử trong danh các video được trả về và hiển thị ra trình phát video.
 
    def setList(self):
        def func(name):
            self.fileName = name
            self.reset = True
            self.des.insert(INSERT, "Switch to file " + name + '\n\n')
        for item in self.videos:
            button = Button(self.frameContainer, text=item, width=20, padx=2, pady=2, command=functools.partial(func,item))
            button.pack(side=TOP)
Class ServerWorker:
•   Hàm processRtspRequest: xử lý request tương ứng với trạng thái INIT và chuyển sang SWITCH.
    def processRtspRequest(self, data):
		. . .
        if requestType == self.LOAD:
            if self.state == self.INIT:
                self.clientInfo['session'] = randint(100000, 999999)
                print("processing LOAD\n")
                self.state = self.SWITCH
                self.replyLoad(self.OK_200, seq[1])
•   Hàm replyLoad: được sử dụng để trả tín hiệu về cho client sau khi gọi request INIT và chuyển sang trạng thái SWITCH.
    def replyLoad(self, code, seq):
        if code == self.OK_200:
            #print("200 OK")
            videos = ','.join(VideoStream.getVideosList())
            reply = 'RTSP/1.0 200 OK\nCSeq: ' + seq + '\nSession: ' + str(self.clientInfo['session']) + '\nVideos: ' + videos
            self.clientInfo['rtspSocket'][0].send(reply.encode())
       
        # Error messages
        elif code == self.FILE_NOT_FOUND_404:
            print("404 NOT FOUND")
        elif code == self.CON_ERR_500:
            print("500 CONNECTION ERROR")
Kết quả thu được thể hiện dưới hình dưới:
 
6.5  Hiện thực FASTER và LOWER request:
Để thay đổi tốc độ chiếu thì ta sẽ tiến hành thay đổi thời gian chờ (mặc định là 0.05s ứng với FPS = 20). Ta sẽ thêm biến speed vào phần khởi tạo
    def __init__(self, clientInfo):
        self.clientInfo = clientInfo
        self.speed = 0.05
Khi nhận được request từ người client thì phía server sẽ thay đổi speed tương ứng
•	Faster: tốc độ tăng gấp đôi nên thời gian chờ giảm một nửa 
•	Lower: tốc độ giảm một nửa nên thời gian chờ tăng gắp đôi

Bên trong hàm processRtsprequest:
        # Process FASTER request
        elif requestType == self.FASTER:
            print("processing FASTER\n")
            self.speed/=2
            self.replyRtsp(self.OK_200, seq[1])
 
        # Process LOWER request
        elif requestType == self.LOWER:
            print("processing LOWER\n")
            self.speed*=2
            self.replyRtsp(self.OK_200, seq[1])
6.6  Hiện thực FORWARD và BACK request:
•	Sau khi load toàn bộ video lên thì ta sẽ lưu trong server và có thể điều chỉnh trực tiếp frames sẽ phát hiện tại thông qua hàm setframe được hiện thực bên trong class videostream:
    def setFrame(self, frameNum):
        self.frameNum = frameNum if frameNum < len(self.frames) else len(self.frames) - 1
 
•	Khi người dùng muốn tua tới (cụ thể là 30 frames) thì client sẽ gửi một request FORWARD tới server cùng với frame mà người dùng đi tới sau khi đã tăng thêm 30 frames 
Bên trong hàm sendRtspRequest:
        elif requestCode == self.FORWARD :
            # Update RTSP sequence number.
            self.rtspSeq+=1
           
            # Update frame
            self.frameNbr +=30
 
            # Write the RTSP request to be sent.
            msg = 'FORWARD ' + self.fileName + ' RTSP/1.0\nCSeq: ' + str(self.rtspSeq) + '\nSession: ' + str(self.sessionId) + '\nFrame: ' + str(self.frameNbr)
           
            # Keep track of the sent request.
            self.requestSent = self.FORWARD
•	Phía server sau khi nhận được request từ client sẽ thay đổi trực tiếp frames hiện tại thành số frame mà server gửi tới và gửi phản hồi về client 
Bên trong hàm processRtsprequest:
        # Process Forward request
        elif requestType == self.FORWARD:
            try:
                frameNum = int(request[3].split(' ')[1])
            except:
                print(data)
            print("processcing Forward\n")
            self.replyRewind(self.OK_200, seq[1], frameNum)
Bên trong hàm processRtsprequest:
    def replyRewind(self, code, seq, frameNum):
        if code == self.OK_200:
            self.clientInfo['videoStream'].setFrame(frameNum)
            reply  = 'RTSP/1.0 200 OK\nCSeq: ' + seq + '\nSession: ' + str(self.clientInfo['session']) + '\nFrame: ' + str(frameNum)
            self.clientInfo["rtspSocket"][0].send(reply.encode())
        elif code == self.FILE_NOT_FOUND_404:
            print("404 NOT FOUND")
        elif code == self.CON_ERR_500:
            print("500 CONNECTION ERROR")

-	Tương tự cho hàm BACK nhưng ta sẽ giảm số frame xuống và nếu số frame nhỏ hơn 0 thì ta sẽ gửi 0.
7 Hướng dẫn sử dụng
7.1 Server
Để có thể sử dụng được ứng dụng, người dùng cần phải có một server. Để có thể mở Server, người dùng nhập lệnh sau vào Command Prompt:
python Server.py [Server_Port]
 
Trong đó Server_Port là cổng mà server sẽ nhận các kết nối RTSP. Cổng RTSP chuẩn là 554 nhưng người dùng cần phải chọn một cổng lớn hơn 1024. Sau khi nhập thành công lệnh trên, giao diện Command Prompt sẽ chờ các kết nối từ Client đến.
7.2 Client
Sau khi Server được mở thành công, ta chạy Client với câu lệnh như sau vào Command Promt:
•   Đối với phần requirements:
python ClientLauncher.py  [Server_Host]  [Server_Port]  [RTP_Port]  [Video_File]
•   Đối với phần extend:
python ClientLauncher.py [Server_Host] [Server_Port] [RTP_Port]
 
Trong đó:
•   Server_Host: là địa chỉ IP của máy mà đang chạy server (trong phạm vi bài tập lớn nhóm sử dụng port là localhost hay "127.0.0.1").
•   Server_Port: là cổng mà server đang nhận tín hiệu.
•   RTT_Port: là cổng mà ở đó Client sẽ nhận RTP packet.
•   Video_File: là tên của video file. Ở trong bài tập lớn này, tên video mặc định là "movie.Mjpeg", nhưng người dùng có thể thao tác chọn video trong danh sách các video có sẵn.
Các nút chức năng của client:
•   Đối với client có đầy đủ nút bấm:
 8 Đánh giá kết quả đạt được
Tuy có một số sai lệch với kế hoạch nhưng kết quả vẫn đạt theo đúng yêu cầu ban đầu nhóm đề ra. Qua bài tập lớn này, nhóm đã tìm hiểu được về các giao thức RTP/RTSP socket. Cụ thể là ở việc truyền, nhận tín hiệu, mã hóa và giải mã tín hiệu, hiện thực được một ứng dụng thực tế cho việc streaming video.


 
 
 
Tài liệu tham khảo
[1]	 Real Time Streaming Protocol (RTSP) , truy cập từ: https://books.google.com.vn/ books?id=5fms2DW7mMUC&pg=PA42&redir_esc=y#v=onepage&q&f=false
[2]	Software Requirements Analysis with Example. truy cập từ https://www.guru99.com/ learn-software-requirements-analysis-with-case-study.html
[3]	RTP: A Transport Protocol for Real-Time Applications, truy cập từ https://datatracker. ietf.org/doc/html/rfc1889
[4]	Session  Description  Protocol  (SDP)  Format   for   Real   Time   Streaming   Proto- col (RTSP) Streams, truy cập từ https://datatracker.ietf.org/doc/html/ draft-marjou-mmusic-sdp-rtsp-00
[5]	RFC 2326, Real Time Streaming Protocol (RTSP), IETF, 1998, pages 28-29.

 
 
 
 
 

