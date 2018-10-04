# mixin
Mixin for classes

{return}
class A {
    componentDidMount(a){
        console.log(1)
    }
}

class B {
    componentDidMount(a){
        console.log(a)
    }
     pepe(a){
        console.log("pepe says "+a)
    }
}


const mixin = (...classes) => {
    class _mixin {}
    let proto = _mixin.prototype
    classes.map(({prototype:p}) => {
        Object.getOwnPropertyNames(p).map((key, index) => {
            let oldFn = proto[key] || (() => {})
            proto[key] = (args) => {
              oldFn(args)
                return p[key](args)
            }
        })
    })

    return _mixin
}
class C extends mixin(A,B) {
    componentDidMount(){
        super.componentDidMount("Something")
        console.log("Something Else")
    }
}
let c = new C()
c.componentDidMount()
c.pepe("whatever")
