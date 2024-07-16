import Cocoa
import SceneKit

class GameViewController: NSViewController {
    
    var sceneView: SCNView!
    var scene: SCNScene!
    var cameraNode: SCNNode!
    var playerNode: SCNNode!
    var groundNode: SCNNode!
    var lightNode: SCNNode!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        setupScene()
        setupCamera()
        setupPlayer()
        addGround()
        addLights()
        
        // Start rendering
        sceneView.isPlaying = true
    }
    
    func setupScene() {
        sceneView = SCNView(frame: view.bounds)
        sceneView.autoresizingMask = [.width, .height]
        view.addSubview(sceneView)
        
        scene = SCNScene()
        sceneView.scene = scene
        scene.physicsWorld.contactDelegate = self
    }
    
    func setupCamera() {
        cameraNode = SCNNode()
        cameraNode.camera = SCNCamera()
        cameraNode.position = SCNVector3(x: 0, y: 5, z: 10)
        scene.rootNode.addChildNode(cameraNode)
    }
    
    func setupPlayer() {
        playerNode = SCNNode()
        let playerGeometry = SCNCapsule(capRadius: 0.25, height: 1.0)
        playerNode.geometry = playerGeometry
        playerNode.position = SCNVector3(x: 0, y: 0.5, z: 0)
        playerNode.physicsBody = SCNPhysicsBody(type: .dynamic, shape: nil)
        playerNode.physicsBody?.categoryBitMask = 1
        playerNode.physicsBody?.contactTestBitMask = 2 // For collision detection
        scene.rootNode.addChildNode(playerNode)
    }
    
    func addGround() {
        groundNode = SCNNode()
        let groundGeometry = SCNFloor()
        groundNode.geometry = groundGeometry
        groundNode.position = SCNVector3(x: 0, y: -0.5, z: 0)
        scene.rootNode.addChildNode(groundNode)
    }
    
    func addLights() {
        lightNode = SCNNode()
        lightNode.light = SCNLight()
        lightNode.light?.type = .omni
        lightNode.position = SCNVector3(x: 0, y: 10, z: 10)
        scene.rootNode.addChildNode(lightNode)
    }
    
    override func keyDown(with event: NSEvent) {
        handleMovement(event)
    }
    
    func handleMovement(_ event: NSEvent) {
        let moveDistance: CGFloat = 1.0
        let rotateAngle: CGFloat = 0.1
        
        switch event.keyCode {
        case 126: // Up arrow
            playerNode.position.z -= moveDistance
        case 125: // Down arrow
            playerNode.position.z += moveDistance
        case 123: // Left arrow
            playerNode.position.x -= moveDistance
        case 124: // Right arrow
            playerNode.position.x += moveDistance
        default:
            break
        }
    }
}

extension GameViewController: SCNPhysicsContactDelegate {
    func physicsWorld(_ world: SCNPhysicsWorld, didBegin contact: SCNPhysicsContact) {
        // Handle collisions or interactions between nodes
    }
}
