|功能描述|知识点|
|:---|:---|
|解析pom文件，获取标签内的值|正则表达式，DomParser|

**1：解析xml 文件获取内容**
```
export const getPomValues = (pomString, keys) => {
    const step1 = String(pomString).replace(/[\n]/g, '');
    const step2 = String(step1).replace(/[\r]/g, '');
    const xmlDom = new DOMParser().parseFromString(step2, 'text/xml');
    const result = {};
    keys.forEach(key => {
        const nodes = xmlDom.getElementsByTagName(key) || [];
        const node = nodes[0];
        if (node) {
            const value = node.innerHTML || '-';
            result[key] = value.trim();
        }
    });
    return result;
};
```
**2：渲染大数据时的单位（unit）
```
/**
     * 为数字加上单位：万或亿
     *
     * 例如：
     *      1000.01 => 1000.01
     *      10000 => 1万
     *      99000 => 9.9万
     *      566000 => 56.6万
     *      5660000 => 566万
     *      44440000 => 4444万
     *      11111000 => 1111.1万
     *      444400000 => 4.44亿
     *      40000000,00000000,00000000 => 4000万亿亿
     *      4,00000000,00000000,00000000 => 4亿亿亿
     *
     * @param {number} number 输入数字.
     * @param {number} decimalDigit 小数点后最多位数，默认为2
     * @return {string} 加上单位后的数字
     */
    addNumberUnit(number, decimalDigit){
        decimalDigit = decimalDigit == null ? 0 : decimalDigit;
        number = number == null ? 0 : number;
        let integer = Math.floor(number);
        let digit = GET_DIGIT(integer);
        // ['个', '十', '百', '千', '万', '十万', '百万', '千万'];
        let unit = [];
        if (digit > 3) {
            let multiple = Math.floor(digit / 8);
            if (multiple >= 1) {
                let tmp = Math.round(integer / Math.pow(10, 8 * multiple));
                unit.push(ADD_WAN(tmp, number, 8 * multiple, decimalDigit));
                for (let i = 0; i < multiple; i++) {
                    unit.push('亿');
                }
                return unit.join('');
            } else {
                return ADD_WAN(integer, number, 0, decimalDigit);
            }
        } else {
            return number;
        }
    }
    ```
