## JS设计思想状态模式

**ES6语法**

    class Mary {
        constructor () {
            this.stateArr = [];
            this.action = {
                run () {},
                jump () {}
            }
        }
        extends (name, fn) {
            this.action[name] = fn;
        }
        exec () {
            const that = this;
            this.stateArr.forEach((item, index) => {
                that.action[item]();
            });
        }
        addState (code) {
            let state = '';
            switch (code) {
                case 0 :
                    state = 'run';
                case 1:
                    state = 'jump';
                case 2:
                    state = 'fire';
                default:
                    return '';
            }

            state && this.stateArr.push(state);
            return this;
        }
    }

    const mary = new Mary();

    mary.extends('fire', () => {});

    window.onkeydown = (e) => {
        const code = e.keyCode;
        mary.addState(code).exec();
    };

**ES5语法**

    function mary () {
        this.stateArr = [];
        this.action = {
            run: function () {

            },
            jump: function () {

            }
        }
    }

    mary.prototype.extends = function (name, fn) {
        this.action[name] = fn;
    };

    mary.prototype.addState = function (code) {
        let state = '';
        switch (code) {
            case 0 :
                state = 'run';
            case 1:
                state = 'jump';
            case 2:
                state = 'fire';
            default:
                return '';
        }

        state && this.stateArr.push(state);
        return this;
    };

    mary.prototype.exec = function () {
        const that = this;
        this.stateArr.forEach((item, index) => {
            that.action[item]();
        });
    };

    var maryOb = new mary();
    maryOb.extends('fire', function () {});

    window.onkeydown = function () {
       var code = event.keyCode;
       maryOb.addState(code).exec();
    };



