# Akka

## This is an example how can we use Akka to define an actor and its receive method

```
import akka.actor.{Props, ActorSystem, Actor}

object SimpleExample extends App {

  class SimpleActor extends Actor {


    def receive: Receive = {
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

## With two actors

```
import akka.actor.Actor.Receive
import akka.actor.{Props, ActorSystem, Actor, ActorRef}

object CountDown extends App {
  case class StartCounting(n:Int,partner:ActorRef)
  case class Count(n:Int)

  class CountDownActor extends Actor{
     def receive: Receive = {
       case StartCounting(n,o) =>
         println(n)
         o ! Count(n-1)
       case Count(n) => if(n>0){
         println(n)
         Thread.sleep(1000)
         sender ! Count(n-1)
       }
         else{
         context.system.shutdown
       }
     }
  }

  val system = ActorSystem("SimpleExample")
  val actor1 = system.actorOf(Props[CountDownActor],"Actor1")
  val actor2 = system.actorOf(Props[CountDownActor],"Actor2")

  actor1 ! StartCounting(10,actor2)
}

```
