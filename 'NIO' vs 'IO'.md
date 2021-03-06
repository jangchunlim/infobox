IO
스트림 지향 : 스트림에서 한 번에 하나 이상의 바이트를 읽는 것
NIO
버퍼 지향 : 데이터는 나중에 처리되는 임의의 버퍼로 읽는 것, 버퍼 이용해 완전히 처리하려면 필요한 모든 데이터가 버퍼에 들어 있는지 확인,
           더 많은 데이터를 읽을 때, 아직 처리하지 않은 버퍼의 데이터를 덮어쓰지 않도록 주의

IO
Blocking : 스레드가 read(), write()를 호출하면 완전히 읽거나 쓰여질 때까지 스레드가 차단됨
NIO
Non-Blocking : 스레드가 read(), write()를 호출해도 읽거나 쓰여질 때까지 기다리지 않고 계속 진행됨(스레드가 차단되지 않음)

NIO
Selectos : 하나의 스레드에서 다중 입력 채널을 관리할 수 있음

channerls : 채널을 통해서 읽고 쓸 수 있음, 비동기적
stream : 일반적으로 단방향으로만 가능

<pre>
<code>
import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.SelectionKey;
import java.nio.channels.Selector;
import java.nio.channels.ServerSocketChannel;
import java.nio.channels.SocketChannel;
import java.util.Iterator;
import java.util.Set;

public class EchoServer {

    private static final String EXIT = "EXIT";

    public static void main(String[] args) throws IOException {
        Selector selector = Selector.open();
        ServerSocketChannel serverSocket = ServerSocketChannel.open();
        serverSocket.bind(new InetSocketAddress("localhost", 3000));
        serverSocket.configureBlocking(false);
        serverSocket.register(selector, SelectionKey.OP_ACCEPT);
        ByteBuffer buffer = ByteBuffer.allocate(256);

        while (true) {
            selector.select();
            Set<SelectionKey> selectedKeys = selector.selectedKeys();
            Iterator<SelectionKey> iter = selectedKeys.iterator();
            while (iter.hasNext()) {

                SelectionKey key = iter.next();

                if (key.isAcceptable()) {
                    register(selector, serverSocket);
                }

                if (key.isReadable()) {
                    answerWithEcho(buffer, key);
                }
                iter.remove();
            }
        }
    }

    private static void answerWithEcho(ByteBuffer buffer, SelectionKey key)
            throws IOException {

        SocketChannel client = (SocketChannel) key.channel();
        client.read(buffer);
        if (new String(buffer.array()).trim().equals(EXIT)) {
            client.close();
            System.out.println("Not accepting client messages anymore");
        }

        buffer.flip();
        client.write(buffer);
        buffer.clear();
    }

    private static void register(Selector selector, ServerSocketChannel serverSocket)
            throws IOException {

        SocketChannel client = serverSocket.accept();
        client.configureBlocking(false);
        client.register(selector, SelectionKey.OP_READ);
        System.out.println("new client connected...");
    }
}
</code>
</pre>
