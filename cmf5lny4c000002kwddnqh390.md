---
title: "Viết Test Class Tự Sinh Bằng Copilot Agent – ClaudeSonnet Trong Salesforce"
datePublished: Thu Sep 04 2025 16:07:45 GMT+0000 (Coordinated Universal Time)
cuid: cmf5lny4c000002kwddnqh390
slug: viet-test-class-tu-sinh-bang-copilot-agent-claudesonnet-trong-salesforce

---

## 1\. Giới thiệu

* Vì sao testclass quan trọng trong Salesforce (coverage, chất lượng code, an toàn khi deploy).
    
* Thách thức: viết test thủ công tốn effort.
    
* Giải pháp: copilot agent ClaudeSonnet – không chỉ sinh test mà còn **deploy + run + fix tự động**.
    

## 2\. Copilot Agent – ClaudeSonnet là gì?

* LLM dạng agent có khả năng:
    
    * Sinh/chỉnh sửa test class.
        
    * Deploy vào org.
        
    * Run test, phân tích coverage.
        
    * Tự động fix cho đến khi đạt yêu cầu.
        
* Điểm mạnh: không dừng ở code generation, mà **thực sự giống một dev tester tự động**.
    

## 3\. Quy trình hoạt động với Copilot Agent – ClaudeSonnet

1. **Chỉnh prompt cho đúng tên testclass**
    
    * Cập nhật trong prompt mẫu tên class thực tế (ví dụ: `Arf_AP_CreditUpsertLogicTest`).
        
    * Đảm bảo mọi rule (≥ 75% coverage, pass 100%, dùng `Arf_TestDataFactory`, comment tiếng Nhật, …) đã đúng theo guideline team.
        
2. **Paste testclass vào Copilot Chat**
    
    * Nếu đã có testclass, dán cả nội dung testclass hiện tại.
        
    * Nếu chưa có, chỉ cần dán Apex class gốc.
        
3. **Add folder context**
    
    * Thêm thư mục context để agent tham khảo:
        
        * `/TestSetup/` → ví dụ cách tạo data chuẩn.
            
        * `/FixPatterns/` → các mẫu sửa lỗi thường gặp.
            
        * `/ValidationRules/` → các rule cần lưu ý khi insert data.
            
    * Agent sẽ dùng context này để tự fix khi gặp lỗi hoặc thiếu coverage.
        
4. **Chỉnh mode sang Agent và model Claude Sonnet**
    
    * Trong Copilot chọn **mode = agentic**.
        
    * Model: **Claude Sonnet** (tối ưu cho coding + test automation).
        

## 4\. Setting cho user trước khi chạy

* **Auto approve cho chat**:  
    → Để agent có thể deploy, run test, fix mà không cần click *Continue* thủ công.
    
* **Response limit = 10000**:  
    → Đảm bảo agent không bị cắt output khi sinh testclass dài hoặc log sửa lỗi nhiều vòng, và không cần click continue nếu response đang bị dài hay copilot working with 1 testclass quá lâu.
    

## 4\. Prompt mẫu

* Ví dụ prompt chi tiết (như bạn đã đưa).
    
* Chỉ rõ rule (không tạo nhiều testclass, không sửa class gốc, dùng `Arf_TestDataFactory`, comment bằng tiếng Nhật, …).
    
* Bắt buộc coverage ≥ 75% & pass 100%.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1757002011037/19c2a95a-3b06-4a07-860b-6e331aa421f3.png align="center")

## Best Practice khi sinh testclass tự động bằng Copilot Agent – ClaudeSonnet

1. **Tận dụng multi–workspace / multi–tab trong VS Code**
    
    * Mỗi tab VS Code chạy một session Copilot Agent.
        
    * Bạn có thể mở song song 4–5 tab, mỗi tab phụ trách 1 testclass khác nhau trong cùng org.
        
    * Như vậy vừa tiết kiệm thời gian, vừa tăng tốc độ đạt coverage cho nhiều class.
        
2. **Quản lý theo org**
    
    * Với một org cụ thể, gom nhóm các class cần test vào cùng workspace.
        
    * Tránh chạy lẫn class từ nhiều org trong một màn hình, vì context và metadata sẽ khác nhau.
        
3. **Theo dõi kết quả song song**
    
    * VS Code cho phép split view → vừa xem Copilot Agent log (deploy/run/fix) vừa xem Salesforce Test Execution result.
        
    * Dễ dàng biết class nào đã đạt coverage ≥ 75% và class nào cần tiếp tục fix.
        
4. **Tái sử dụng folder context**
    
    * Tạo chung 1 `context` folder cho cả workspace (chứa TestDataFactory sample, FixPatterns, ValidationRules).
        
    * Khi chạy 4–5 session song song, tất cả agent đều tham chiếu được → giảm lỗi lặp lại.
        

## 5\. Sức mạnh tham khảo tri thức

* **Folder context**:
    
    * Lưu các fix pattern (vd: cách xử lý exception test, async test với `Test.startTest/stopTest`, picklist validation).
        
    * AI dùng làm reference để tự động sửa lỗi lặp lại.
        
* **Knowledge base (KB)**:
    
    * Có thể tích hợp với GitHub Copilot KB hoặc knowledge base riêng.
        
    * Bạn tổ chức KB theo module (Trigger, Service, Batch, Queueable, TestDataFactory).
        
    * Khi test fail hoặc coverage thiếu, ClaudeSonnet tìm pattern trong KB để fix.
        
* Kết quả: agent càng chạy nhiều, càng “thông minh” và ổn định vì luôn có tri thức tham chiếu.
    

## 6\. Điểm mạnh

* Tự động hóa hoàn toàn (code → deploy → run → fix).
    
* Sử dụng context để giảm lặp lỗi.
    
* Bám sát metadata thực tế và KB nội bộ.
    
* Tăng tốc viết test nhưng vẫn đảm bảo chuẩn dev team.
    

## 7\. Kinh nghiệm viết prompt

* Chỉ rõ environment.
    
* Yêu cầu coverage ≥ 75% & pass 100%.
    
* Bắt buộc tham chiếu `Arf_TestDataFactory`.
    
* Hướng dẫn agent tham khảo folder context/KB để sửa lỗi.
    
* Nhấn mạnh việc assert & comment chi tiết.
    

## 8\. Ưu – nhược điểm

✅ Ưu điểm:

* Giảm effort viết test.
    
* Tự động fix nhờ context + KB.
    
* Đảm bảo chất lượng cao, phù hợp với metadata thực tế.
    

⚠️ Nhược điểm:

* Cần set up KB + folder context ban đầu.
    
* Phụ thuộc vào environment connection.
    
* Dev vẫn cần review logic nghiệp vụ.
    

## 9\. Kết luận

* ClaudeSonnet agent không chỉ viết test, mà còn học từ **context + KB** để fix tự động.
    
* Prompt tốt + KB phong phú = testclass chất lượng, ít vòng lặp.
    
* Đây là hướng đi giúp dev Salesforce tập trung vào **logic & business**, để AI lo phần coverage.