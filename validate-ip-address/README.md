# Leetcode: Validate IP Address     :BLOG:Medium:


---

Validate IP Address  

---

Similar Problems:  
-   [IP to CIDR](https://brain.dennyzhang.com/ip-to-cidr)
-   [Tag: #manydetails](https://brain.dennyzhang.com/tag/manydetails)

---

Write a function to check whether an input string is a valid IPv4 address or IPv6 address or neither.  

IPv4 addresses are canonically represented in dot-decimal notation, which consists of four decimal numbers, each ranging from 0 to 255, separated by dots ("."), e.g.,172.16.254.1;  

Besides, leading zeros in the IPv4 is invalid. For example, the address 172.16.254.01 is invalid.  

IPv6 addresses are represented as eight groups of four hexadecimal digits, each group representing 16 bits. The groups are separated by colons (":"). For example, the address 2001:0db8:85a3:0000:0000:8a2e:0370:7334 is a valid one. Also, we could omit some leading zeros among four hexadecimal digits and some low-case characters in the address to upper-case ones, so 2001:db8:85a3:0:0:8A2E:0370:7334 is also a valid IPv6 address(Omit leading zeros and using upper cases).  

However, we don't replace a consecutive group of zero value with a single empty group using two consecutive colons (::) to pursue simplicity. For example, 2001:0db8:85a3::8A2E:0370:7334 is an invalid IPv6 address.  

Besides, extra leading zeros in the IPv6 is also invalid. For example, the address 02001:0db8:85a3:0000:0000:8a2e:0370:7334 is invalid.  

Note: You may assume there is no extra space or special characters in the input string.  

Example 1:  

    Input: "172.16.254.1"
    
    Output: "IPv4"
    
    Explanation: This is a valid IPv4 address, return "IPv4".

Example 2:  

    Input: "2001:0db8:85a3:0:0:8A2E:0370:7334"
    
    Output: "IPv6"
    
    Explanation: This is a valid IPv6 address, return "IPv6".

Example 3:  

    Input: "256.256.256.256"
    
    Output: "Neither"

Explanation: This is neither a IPv4 address nor a IPv6 address.  

Github: [challenges-leetcode-interesting](https://github.com/DennyZhang/challenges-leetcode-interesting/tree/master/validate-ip-address)  

Credits To: [leetcode.com](https://leetcode.com/problems/validate-ip-address/description/)  

Leave me comments, if you have better ways to solve.  

    ## Blog link: https://brain.dennyzhang.com/validate-ip-address
    ## Basic Ideas:
    ##
    ## Assumptions: 
    ##  0.0.0.0 is valid
    ##  255.255.255.255 is valid
    ##  0101:db8:85a3:0:0:8A2E:0370:7334
    ## Complexity:
    class Solution:
        def validIPAddress(self, IP):
            """
            :type IP: str
            :rtype: str
            """
            self.invalid = "Neither"
            if "." in IP:
                return self.validIPv4Address(IP)
            elif ":" in IP:
                return self.validIPv6Address(IP)
            return self.invalid
    
        def validIPv4Address(self, IP):
            """
            :type IP: str
            :rtype: str
            """
            l = IP.split('.')
            if len(l) != 4: return self.invalid
            for item in l:
                if item.isdigit() is False: return self.invalid
                if item != '0' and item[0] == '0': return self.invalid
                v = int(item)
                if v<0 or v>255: return self.invalid
            return "IPv4"
    
        def validIPv6Address(self, IP):
            """
            :type IP: str
            :rtype: str
            """
            l = IP.split(':')
            if len(l) != 8: return self.invalid
            for item in l:
                if len(item) > 4: return self.invalid
                if item.isalnum() is False: return self.invalid
                # unify the string
                item = item.lower()
                item = item.lstrip('0')
                if item == '': item = '0'
                for ch in item:
                    if ch.isdigit() is False and ch not in 'abcdef':
                        return self.invalid
            return "IPv6"