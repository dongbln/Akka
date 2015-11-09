# Akka

## This is an example how can we use Akka to define an actor and its receive method

```
import akka.actor.{Props, ActorSystem, Actor}

object SimpleExample extends App {

  class SimpleActor extends Actor {


    override def receive: Receive = {
      case s: String => println("String " + s)
      case i: Int => println("Int" + i)
      case _ => println("Unknown message")
    }

    val system = ActorSystem("TestSystemExample")
    val actor = system.actorOf(Props[SimpleActor], "First Actor")

    // Sending the messages

    actor ! "Hi"
    actor ! 42
    actor ! 'v'

    system.shutdown()


  }

}

```
