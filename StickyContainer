import React, { Component } from 'react'

export default class StickyContainer extends Component {
  static propTypes = {
    children:React.PropTypes.any
  }
  state = {
    isBottom : false,
    width:0,
    top:0,
    sticktemplateTop:0,
    initialTop:0
  }

  on (events, callback) {
    events.forEach((evt) => {
      window.addEventListener(evt, callback)
    })
  }
  off (events, callback) {
    events.forEach((evt) => {
      window.removeEventListener(evt, callback)
    })
  }
  componentDidMount () {
    const initialTop = this.refs.template.getBoundingClientRect().top
    this.setState({
      isBottom:false,
      sticktemplateTop:initialTop,
      initialTop:initialTop
    })
    this.on(['resize', 'scroll', 'touchstart', 'touchmove',
      'touchend', 'pageshow', 'load'], this.recomputeState(this))
  }

  componentWillUnmount () {
    this.off(['resize', 'scroll', 'touchstart', 'touchmove',
      'touchend', 'pageshow', 'load'], this.recomputeState)
  }

  recomputeState = (component) => (event) => {
    const stickBody = component.refs.children.getBoundingClientRect()
    const sticktemplate = component.refs.template.getBoundingClientRect()
    const { isBottom, top, sticktemplateTop, width } = this.state
    // Change the width
    if (width !== sticktemplate.width) {
      this.setState({ width:sticktemplate.width })
    }
    // Judge whether small size screen or not
    if (isBottom && window.innerWidth < 640) {
      this.setState({ isBottom : false })
    }
    // Judge if stick
    if (!isBottom && window.innerHeight >= stickBody.bottom) {
      this.setState({ isBottom:true, top:window.innerHeight - stickBody.height })
    }
    // Judge if unstick
    if (isBottom && stickBody.top <= sticktemplateTop.top) {
      this.setState({ isBottom:false, top:sticktemplateTop.top })
    }

    if (isBottom) {
      if (sticktemplateTop < sticktemplate.top) {
        if (top > sticktemplate.top && top < this.state.initialTop) {
          this.setState((prevState) => ({
            top:prevState.top + sticktemplate.top - prevState.sticktemplateTop
          }))
        } else {
          this.setState({ top:this.state.initialTop })
        }
      } else {
        if (top > window.innerHeight - stickBody.height) {
          this.setState((prevState) => ({
            top:prevState.top + sticktemplate.top - prevState.sticktemplateTop
          }))
        } else {
          this.setState({ top:window.innerHeight - stickBody.height })
        }
      }
    }
    this.setState({ sticktemplateTop: sticktemplate.top })
  }

  render () {
    const style = this.state.isBottom
      ? { position:'fixed', top:this.state.top, width:this.state.width } : {}
    return (
      <div>
        <div ref='template' className='template' />
        <div ref='children' className='stick_body'
          style={{ ...style, transform:'translateZ(0px)' }}>
          {this.props.children}
        </div>
      </div>
    )
  }
}
