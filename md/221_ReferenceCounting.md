
# 引用计数法


    对象维护一个引用计数器，
    如果一个对象增加了一个引用与之相连，则将引用值加一，反之减一。
    如果引用值变为0，则说明该对象已经被废弃。

# 优点

    实现简单

# 缺点
    
    如果一个对象A持有对象B，而对象B也持有一个对象A，这就是循环引用
    这种情况下A与B的counter恒大于1，会使得GC永远无法回收这两个对象。

# 示例

    循环引用实例
    
    
    public class ReferenceCountingGC {

        public Object instance = null;

        private static final int _1MB = 1024 * 1024;

        /** * 这个成员属性的唯一意义就是占点内存，以便在能在GC日志中看清楚是否有回收过 */
        private byte[] bigSize = new byte[2 * _1MB];

        public static void testGC() {
            ReferenceCountingGC objA = new ReferenceCountingGC();
            ReferenceCountingGC objB = new ReferenceCountingGC();
            objA.instance = objB;
            objB.instance = objA;

            objA = null;
            objB = null;

            // 假设在这行发生GC，objA和objB是否能被回收？
            System.gc();
        }
        public static void main(String[] args) {
            testGC();
        }
    }




 
