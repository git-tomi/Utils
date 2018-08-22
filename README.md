# Utils
常用util
收集常用的util包
1：解析xml 文件获取内容
\n
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
