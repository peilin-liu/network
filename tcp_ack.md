# tcp
## sack

### Sack-Permitted Option
This two-byte option may be sent in a SYN by a TCP that has been
   extended to receive (and presumably process) the SACK option once the
   connection has opened.  It MUST NOT be sent on non-SYN segments.

       TCP Sack-Permitted Option:

       Kind: 4

       +---------+---------+
       | Kind=4  | Length=2|
       +---------+---------+
很多博客上都说这里各占四个字节，误认子弟。这里英文描述摘自 rfc 2018, 很明确说这里是占2个字节的。

### Sack Option Format
   The SACK option is to be used to convey extended acknowledgment
   information from the receiver to the sender over an established TCP
   connection.

       TCP SACK Option:

       Kind: 5

       Length: Variable

                         +--------+--------+
                         | Kind=5 | Length |
       +--------+--------+--------+--------+
       |      Left Edge of 1st Block       |
       +--------+--------+--------+--------+
       |      Right Edge of 1st Block      |
       +--------+--------+--------+--------+
       |                                   |
       /            . . .                  /
       |                                   |
       +--------+--------+--------+--------+
       |      Left Edge of nth Block       |
       +--------+--------+--------+--------+
       |      Right Edge of nth Block      |
       +--------+--------+--------+--------+

因为option 字段有40 个自己长度，所以 40 - 2(option 描述) = 38 个自己，还能提供 4 * 8 个字节来描述sack.
可以描述四个 sack 段，但是有下列场景的存在，导致可能只能 有三个描述段

   A SACK option that specifies n blocks will have a length of 8*n+2
   bytes, so the 40 bytes available for TCP options can specify a
   maximum of 4 blocks.  It is expected that SACK will often be used in
   conjunction with the Timestamp option used for RTTM [Jacobson92],
   which takes an additional 10 bytes (plus two bytes of padding); thus
   a maximum of 3 SACK blocks will be allowed in this case.
   
网上很多文章在这点上也都是没有提及。

rfc 协议链接地址
https://www.rfc-editor.org/rfc/rfc2018
