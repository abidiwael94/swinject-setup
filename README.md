# swinject-setup

final class AppContainer {
    static let shared = AppContainer()
    let container: Container
    
    init() {
        container = Container()
        registerAssemblies()
    }
    
    private func registerAssemblies() {
        let assemblies: [Assembly] = [
            CatAssembly(),
            DogAssembly()
        ]
        assemblies.forEach { $0.assemble(container: container) }
    }
    
    func resolve<T>(_ type: T.Type) -> T {
        guard let service = container.resolve(T.self) else {
            fatalError("Dependency of type \(T.self) is not registered!")
        }
        return service
    }
    
}


final class CatAssembly: Assembly {
    func assemble(container: Container) {
        container.register(CatService.self) { _ in CatServiceImpl() }
        
        container.register(CatRepository.self) { resolver in
            CatRepositoryImpl(catService: resolver.resolve(CatService.self)!)
        }
        
        container.register(getCatUseCase.self) { resolver in
            getCatUseCaseImpl(catRepository: resolver.resolve(CatRepository.self)!)
        }
        
        container.register(CatListViewModel.self) { resolver in
            CatListViewModel(getCatUseCase: resolver.resolve(getCatUseCase.self)!)
        }
        
    }
}

final class DogAssembly: Assembly {
    func assemble(container: Container) {
        
    }
}

